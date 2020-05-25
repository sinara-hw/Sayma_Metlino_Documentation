Gateware configurations
=======================

\todo[inline]{mathrm problem}
\paragraph{Spline parametrization}\label{spline-parametrization}

\emph{This is inherited from the
	\href{http://pdq2.readthedocs.io/en/latest/}{pdq2 documentation}}

The method of compression is a polynomial basis spline (B-spline). The
data consists of a sequence of knots. Each knot is described by a
duration \texttt{\$\textbackslash{}Delta\ t\$} and spline coefficients
\texttt{\$u\_\{n\}\$} up to order \texttt{\$k\$}. If the knot is
evaluated starting at time \texttt{\$t\_\{0\}\$}, the output
\texttt{\$u(t)\$} for
\texttt{\$t\textbackslash{}in{[}t\_\{0\},t\_\{0\}+\textbackslash{}Delta\ t{]}\$}
is
\texttt{\$\$u(t)=\textbackslash{}sum\_\{n=0\}\^{}\{k\}\textbackslash{}frac\{u\_\{n\}\}\{n!\}(t-t\_\{0\})\^{}\{n\}=u\_\{0\}+u\_\{1\}(t-t\_\{0\})+\textbackslash{}frac\{u\_\{2\}\}\{2\}(t-t\_\{0\})\^{}\{2\}+...\$\$}
A sequence of such knots describes a spline waveform. From one discrete
time \texttt{\$i\$} to the next \texttt{\$i+1\$} each accumulator
\texttt{\$v\_\{n,i\}\$} is incremented by the value of the next higher
order accumulator: \texttt{\$\$v\_\{n,i+1\}=v\_\{n,i\}+v\_\{n+1,i\}\$\$}
For a cubic spline the mapping between accumulators' initial values
\texttt{\$v\_\{n,0\}\$} and the polynomial derivatives or spline
coefficients \texttt{\$u\_\{n\}\$} can be done off-line and must take
into consideration the finite time step size
\texttt{\$\textbackslash{}tau\$}. The data for each knot is described by
the integer duration
\texttt{\$T=\textbackslash{}Delta\ t/\textbackslash{}tau\$} and the
initial values \texttt{\$v\_\{n,0\}\$}. This representation allows both
transient large-bandwidth waveforms and slow but smooth large duty cycle
waveforms to be described very efficiently.

\paragraph{Waveform parametrization}\label{waveform-parametrization}

The gateware will support at least 8 independent channels.

Each channel emits waveforms of the general parametrization:
\texttt{\$\$z=\textbackslash{}left(\ a\_1e\^{}\{i(f\_1\ t+p\_1)\}\ +\ a\_2e\^{}\{i(f\_2\ t+p\_2)\}\textbackslash{}right)e\^{}\{i(f\_0t+p\_0)\}\$\$}
\texttt{\$\$o=u+b\_0\textbackslash{}mathrm\{Re\}(z)+b\_1\textbackslash{}mathrm\{Im\}(z\^{}\textbackslash{}prime)\$\$}

\begin{itemize}
	\item
	\texttt{\$o\$} is the (real valued) output of a channel
	\item
	\texttt{\$z\$} is the complex-valued output of the ``generator''
	associated with each channel
	\item
	\texttt{\$z\^{}\textbackslash{}prime\$} is the complex-valued output
	from the generator of each channel's ``buddy'' channel. Two adjaccent
	channels form a buddy pair. This enables seamless usage of the complex
	data path features in DACs, complex (IQ) analog modulation, and yields
	``four-tone'' support on IQ channels for free.
	\item
	\texttt{\$u\$} and \texttt{\$a\$} are 16-bit cubic (third order)
	spline interpolators
	\item
	\texttt{\$p\$} are 16-bit constant (zeroth order) spline interpolators
	\item
	\texttt{\$f\$} are 48-bit linear (first order) interpolators
	\item
	\texttt{\$b\$} are switches (\texttt{\$1\$} or \texttt{\$0\$})
\end{itemize}

\paragraph{Datapath details}\label{datapath-details}

\begin{itemize}
	\item
	\texttt{\$f\_\textbackslash{}mathrm\{DATA\}\textbackslash{}geq1\textbackslash{},\textbackslash{}mathrm\{GHz\}\$}.
	Exact clock speed is TBD and depends on simultaneously meeting
	hardware constraints and an integer relationship with the RTIO clock
	and physics/noise requirements.
	\item
	Oscillator \texttt{\$f\_0,p\_0\$} is sampled at
	\texttt{\$f\_\{DATA\}\$}
	\item
	Interpolators are updated and interpolate at
	\texttt{\$f\_\textbackslash{}mathrm\{DATA\}/k\$} with \texttt{\$k\$}
	typically 4 or 8 and
	\texttt{\$f\_\textbackslash{}mathrm\{DATA\}/k\ \textbackslash{}geq\ 125\textbackslash{},\textbackslash{}mathrm\{MHz\}\$}
	\item
	Oscillators \texttt{\$f\_\{1,2\},p\_\{1,2\}\$} are sampled at
	\texttt{\$f\_\{DATA\}/k\$}
	\item
	All amplitude summing junctions shall implement saturating summation
	to prevent wrap-around.
	\item
	All amplitude summing junctions shall implement configurable and
	guaranteed gateware low-high limiters.
	\item
	All amplitude summing junctions shall register saturation events.
	\item
	To up-sample the data from the \texttt{\$f\_1\$},\texttt{\$f\_2\$}
	oscillators by \texttt{\$k\$} before passing it into the
	\texttt{\$f\_0\$} oscillator, a CIC filter of order TBD shall be
	implemented for anti-aliasing. CIC filters are linear phase.
	\item
	To implement further anti-aliasing, a symmetric (thus linear phase)
	FIR filter with TBD taps (FPGA DSP resource limits) shall be
	implemented after the CIC filter.
	\item
	All spline interpolators and the total channel output shall be
	monitored by the ARTIQ channel monitoring infrastructure.
	\item
	All spline interpolators shall support ARTIQ injection/override.
\end{itemize}

\paragraph{Clocking and
	synchronization}\label{clocking-and-synchronization}

\begin{itemize}
	\item
	Timestamps for spline knot scheduling are at least 62 bit wide.
	\item
	Spline knots have 16-bit dynamic range in time.
	\item
	In order to support slower sweeps with sparser spline knots, the
	dynamic range of the spline coefficients can be extended using time
	stretcher. It decelerates the spline evolution/interpolation rate by a
	factor of \texttt{\$2\^{}E\$}.
	\item
	Waveform output shall be with deterministic latency with respect to
	the RTIO clock:
	
	\begin{itemize}
		\item
		across channels on the same card (to within DAC chip specification)
		\item
		across cards in the same rack (to within DAC chip and intra-rack
		DRTIO clock sycnchronization)
		\item
		across racks controlled by the same core device (to within DAC chip
		and DRTIO clock synchronization)
	\end{itemize}
	\item
	Each card can be clocked by an internal DAC clock derived from the
	RTIO clock or by an external DAC clock.
	\item
	When an external DAC clock is used, the waveform synchronization is
	ensured to within one DAC clock cycle (or the limit of the DAC chip
	whichever is higher) but below that depends on the phase of the
	external DAC clock.
	\item
	All spline knot interpolators can be updated independently (and also
	simultaneously) of each other.
	\item
	All spline interpolator latencies from the internal ``RTIO clock
	reference plane'' to the DAC output are matched and deterministic.
	Channel and board latencies are matched and deterministic (see above).
	\item
	Minimum spline knot duration is
	\texttt{\$k/f\_\textbackslash{}mathrm\{DATA\}\$}.
\end{itemize}

\paragraph{Phase update modes}\label{phase-update-modes}

The phase accumulator of the DDS cores can be updated in multiple
different modes during a phase and/or frequency update.

\begin{itemize}
	\item
	relative phase update:
	\texttt{\$q\^{}\textbackslash{}prime(t)\ =\ q(t\^{}\textbackslash{}prime)\ +\ (p\^{}\textbackslash{}prime\ -\ p)\ +\ (t\ -\ t\^{}\textbackslash{}prime)\ f\^{}\textbackslash{}prime\$}
	\item
	absolute phase update:
	\texttt{\$q\^{}\textbackslash{}prime(t)\ =\ p\^{}\textbackslash{}prime\ +\ (t\ -\ t\^{}\textbackslash{}prime)\ f\^{}\textbackslash{}prime\$}
	\item
	phase coherent update:
	\texttt{\$q\^{}\textbackslash{}prime(t)\ =\ p\^{}\textbackslash{}prime\ +\ (t\ -\ T)\ f\^{}\textbackslash{}prime\$},
	where
	\item
	\texttt{\$q\$}/\texttt{\$q\^{}\textbackslash{}prime\$}: old/new phase
	accumulator
	\item
	\texttt{\$p\$}/\texttt{\$p\^{}\textbackslash{}prime\$}: old/new phase
	offset
	\item
	\texttt{\$f\^{}\textbackslash{}prime\$}: new frequency
	\item
	\texttt{\$t\^{}\textbackslash{}prime\$}: timestamp of setting new
	\texttt{\$p\$},\texttt{\$f\$}
	\item
	\texttt{\$T\$}: ``origin'' timestamp: beginning of experiment, boot of
	device, or arbitrary
	\item
	\texttt{\$t\$}: running time
\end{itemize}

Relative phase updates are called ``continuous phase mode'' and coherent
updates are called ``tracking phase mode'' by some. Phase coherent
updates can be mapped (in software/runtime) to absolute phase updates by
transforming
\texttt{\$p\^{}\textbackslash{}prime\ \textbackslash{}longrightarrow\ p\^{}\textbackslash{}prime\ +\ (t\^{}\textbackslash{}prime\ -\ T)\ f\^{}\textbackslash{}prime\$}.
Since phase coherent updates require large multiplications is is
questionable whether they can and should be implemented in gateware.

It is questionable whether phase coherent updates should or even can be
supported for sweeping \texttt{\$p\$}/\texttt{\$f\$}. They can be
supported for the modulation inputs (see below).

\paragraph{Modulation by RTIO}\label{modulation-by-rtio}

To each spline interpolator (any of the nine \texttt{\$f,p,a,u\$} in the
waveform parametrization) a modulation (summarized as
\texttt{\$e\_\textbackslash{}mathrm\{RTIO\}\$}) by a separate RTIO
channel can be applied.

\begin{itemize}
	\item
	The modulation is an additive offset for frequency and phase
	(\texttt{\$f,p\$}) and a multiplicative offset for amplitudes
	(\texttt{\$u,a\$}).
	\item
	The modulation is times like any other (non-interpolating) RTIO event,
	i.e. \texttt{\$\textbackslash{}leq\ 8\$}ns time resolution and has the
	same value resolution as the spline interpolator it modulates.
	\item
	Default values are 0 for frequency and phase modulation
	(\texttt{\$f,p\$}) and 1 for amplitude modulation (\texttt{\$u,a\$}).
	\item
	Modulation is normalized to full scale.
\end{itemize}

\paragraph{Modulation by local DSP}\label{modulation-by-local-dsp}

In addition to RTIO modulation
\texttt{\$e\_\textbackslash{}mathrm\{RTIO\}\$} there is ``local DSP''
modulation input to each spline interpolator.

\begin{itemize}

	\item
	Same specifications and semantics as the RTIO modulation.
\end{itemize}

\paragraph{Local DSP}\label{local-dsp}

A fully reconfigurable local DSP fabric with multiple IIR filters shall
be included. The DSP switchyard supports servoing applications of
various types.

\begin{itemize}

	\item
	See \href{https://github.com/jordens/redpid}{redpid} for a rough
	feature set.
\end{itemize}

\paragraph{Runtime and kernel
	interface}\label{runtime-and-kernel-interface}

\begin{itemize}
	\item
	Spline knot sequences can be generated off-line and embedded in ARTIQ
	experiments.
	\item
	Spline knot sequences can be generated at compile time.
	\item
	Spline knot sequences can be embedded into ARTIQ experiments and
	emitted to from the core device to the DRTIO channels during the
	experiments.
	\item
	Spline knot sequences can be computed dynamically on core device.
	\item
	Instead of emitting them directly to the DRTIO channel, spline knot
	sequences can be emitted into a named DMA context which stores the
	RTIO events in memory (either on the core device or right at the DRTIO
	channel in the card's DRAM) for later recall.
	\item
	Stored, named DMA segments can be replayed by name.
	\item
	Given enough slack to transmit DRTIO events and fill the channel FIFOs
	(from core device or from any DMA source), all boards, all channels,
	all splines can burst \texttt{\$\textbackslash{}geq128\$} knots each
	at \texttt{\$\textbackslash{}geq125\$}MHz (BRAM FIFO limited). This is
	independent of whether the events are computed dynamically, off-line,
	embedded, reside in core device DRAM or remote DRAM.
	\item
	When sourcing waveforms from core device memory, the sustained
	aggregated spline knot rate across all interpolators is
	\texttt{\$\textbackslash{}geq2\$}MHz.
	\item
	Sourcing from remote DRTIO DMA the spline knot rate per board
	(aggregated over all channels and all interpolators on that board) is
	TBD MHz sustained for TBD knots (DRAM limited).
	\item
	Supports setting \texttt{\$e\_\textbackslash{}mathrm\{DRTIO\}\$} using
	standard DRTIO events.
	\item
	Supports configuring the DAC through RTIO-SPI
	\item
	Utility functions shall be made available to users for processing
	spline waveforms (scaling in value and time, resampling).
	\item
	Given a periodically sampled waveform (vector of values) routines
	shall
	
	\begin{itemize}
		\item
		generate a spline waveform with a fixed knot duration
		\item
		generate a spline waveform with specified knot count and variable
		knot duration
		\item
		generate a spline waveform with minimal knot count and specified RMS
		error
	\end{itemize}
	\item
	given user-supplied spline waveform routines shall
	
	\begin{itemize}
		\item
		generate a periodically sampled waveform (vector of values) with
		user specified resolution
		\item
		determine validity (in-range)
	\end{itemize}
\end{itemize}

\paragraph{Test Cases}\label{test-cases}

ARTIQ Python programs demonstrating the following will be provided.

\begin{enumerate}
	\def\labelenumi{\arabic{enumi}.}
	\item
	Simultaneous generation of two-tone waveforms on 8 DAC channels where
	\texttt{\$f\_\{1\}=f\_\{0\}+\textbackslash{}Delta\$} and
	\texttt{\$f\_\{2\}=f\_\{0\}-\textbackslash{}Delta\$} where
	\texttt{\$f\_\{0\}=200\$}~MHz and
	\texttt{\$\textbackslash{}Delta={[}0,50{]}\$}~MHz.
	\item
	Playing a spline knot sequence demonstrating each spline interpolator
	in turn.
	\item
	Replaying a 128 knot two-tone amplitude sequence from remote DMA.
	\item
	Phase/frequency/amplitude shifting that sequence using
	\texttt{\$e\_\textbackslash{}mathrm\{DRTIO\}\$}.
	\item
	Demonstrate relative and absolute phase mode.
	\item
	Demonstrate deterministic channel alignment to one DAC clock cycle.
	\item
	Demonstrate external and internal clocking.
\end{enumerate}

\subsubsection{Sayma SAWG data rate
	constraints}\label{sayma-sawg-data-rate-constraints}

The fast smart arbitrary waveform channels require a significant amount
of logic resources but also necessitate fulfilling several interacting
constraints on operating frequencies and clock ratios.

For the DAC channel data rate
\texttt{\$f\_\textbackslash{}mathrm\{DATA\}\$} on the JESD204B link, the
following rules need to be observed.

\begin{itemize}
	\item
	\texttt{\$t\_\textbackslash{}mathrm\{DATA\}\ =\ t\_\textbackslash{}mathrm\{RTIO\textbackslash{}\_FINE\}\$}.
	DAC samples need to mesh with RTIO timestamps (e.g.~RF switches on
	TTLs and SYS\_REF tagging), otherwise DAC timing is not
	sample-accurate and samples will beat around RTIO timestamps. The RTIO
	timestamp granularity is a global design variable of an ARTIQ DRTIO
	fabric instance. The granularity does not need to be 1ns and can
	easily be altered globally, but it needs to be the same across the
	entire DRTIO fabric. If e.g.~the core device has a coarse clock of
	125MHz and the high resolution TTL provide three more bits of
	resolution, then the fine timestamp granularity needs to be 1ns (or an
	integer submultiple) everywhere.
	\item
	\texttt{\$t\_\textbackslash{}mathrm\{SLOWDDS\}/k\ =\ t\_\textbackslash{}mathrm\{FASTDDS\}\ =\ t\_\textbackslash{}mathrm\{DATA\}\$}
	with \texttt{\$k\$} a power of two. The accumulator phasing and
	datapath parallelization methods that allow generating multiple
	samples in a single clock cycle only work for powers of two.
	\item
	\texttt{\$t\_\textbackslash{}mathrm\{SLOWDDS\}\$} can potentially be
	as low as 4ns on Kintex 7 with speed grade 2 or better, certainly as
	low as 5ns. The possibility of 4ns fabric timing would need to be
	explored and verified.
	\item
	\texttt{\$t\_\textbackslash{}mathrm\{SLOWDDS\}\ =\ m\ t\_\textbackslash{}mathrm\{RTIO\textbackslash{}\_FINE\}\$}:
	The spline interpolators, RTIO updates, and the slow DDS should mesh
	with the fine timestamp (e.g.~RF switches on TTLs).
	\item
	\texttt{\$t\_\textbackslash{}mathrm\{SLOWDDS\}\ =\ p\ t\_\textbackslash{}mathrm\{RTIO\}\$}:
	The spline interpolators, RTIO updates, and the slow DDS should mesh
	with the coarse timestamp (e.g.~relative to RF switches on coarse
	TTLs). \texttt{\$p\$} is a power of two in the current ARTIQ
	architecture.
	\item
	\texttt{\$f\_\textbackslash{}mathrm\{DATA\}\ \textbackslash{}leq\ 1.09\textbackslash{},\textbackslash{}mathrm\{GHz\}\$}
	or even  1.03,{GHz}\$ for typical DAC and FPGA transciever line rate.
\end{itemize}

The DAC sample rate \texttt{\$f\_\textbackslash{}mathrm\{DAC\}\$} after
interpolation and up-sampling from
\texttt{\$f\_\textbackslash{}mathrm\{DATA\}\$} needs to satisfy:

\begin{itemize}
	\item
	\texttt{\$f\_\textbackslash{}mathrm\{DATA\}\ \textbackslash{}leq\ 2.4\textbackslash{},\textbackslash{}mathrm\{GHz\}\$}:
	Typical DAC sample rate
	\item
	\texttt{\$f\_\textbackslash{}mathrm\{DAC\}\ =\ q\ f\_\textbackslash{}mathrm\{DATA\}\$}
	with
	\texttt{\$q\ \textbackslash{}in\ \textbackslash{}\{1,\ 2,\ 4,\ 8\textbackslash{}\}\$}:
	Available interpolation options
\end{itemize}

\paragraph{Logic and RAM}\label{logic-and-ram}

\begin{itemize}
	\item
	ARTIQ device CPU(s) and miscellaneous logic resources provide a good
	estimate for the additional logic required to support DRTIO. The
	kc705-nist\_qc2 design occupies 23k LUT and 5Mb BRAM. The
	pipistrello-nist\_qc1 design uses 15k LUT and 1Mb BRAM (on a slightly
	different architecture).
	\item
	parallelized FIR: 4 channel, 4x parallelism, 30 taps: 240 DSP
	\item
	parallelized HBF + tricks: 4 channel, 4x parallelism, 30 taps: 120 DSP
	\item
	RTIO FIFOs: 4 channel, 128 knots per RTIO channel: 4Mb
	\item
	PID, extrapolating from redpid (xc7z010): 2 channel 125MHz ADC/DAC +
	misc DSP, full servo crossbar matrix: 13 kLUT, 50 DSP
\end{itemize}

Several design studies were performed for different configurations of
the Sayma SAWG channels:

\begin{itemize}
	\item
	Sayma initial SAWG on kc705: 2 channel, 8x parallelism, 125MHz: 28k
	LUT
	\item
	Sayma advanced draft SAWG on kc705: 4 channel, 4x parallelism, 200MHz:
	33k LUT
	\item
	Sayma advanced draft SAWG on kc705: 4 channel, 8x parallelism, 125MHz:
	53k LUT
	\item
	Sayma advanced draft SAWG on kc705: 8 channel, 8x parallelism, 125MHz:
	106k LUT
	\item
	Sayma advanced draft SAWG on kcu105: 4 channel 4x parallelism, 200MHz:
	33k LUT
\end{itemize}

\paragraph{Data and sample rates}\label{data-and-sample-rates}

\textbf{Somewhere in the Sayma docs, we should have a page about clock
	distribution, giving users an overview of the different constrains that
	exist for clocking. This section should be merged into that and/or the
	SAWG docs.} The following choices for data rates and lanes appear to be
interesting (BW: bandwidth; SSB: single sideband; DSB: dual sideband;
``size'': resource usage in units of 13k LUTs per channel):

\begin{longtable}[]{@{}lllllll@{}}

	\begin{minipage}[b]{0.14\columnwidth}\raggedright\strut
		\texttt{\$f\_\textbackslash{}mathrm\{DAC\}\$}\strut
	\end{minipage} & \begin{minipage}[b]{0.15\columnwidth}\raggedright\strut
	\texttt{\$f\_\textbackslash{}mathrm\{DATA\}\$}\strut
\end{minipage} & \begin{minipage}[b]{0.09\columnwidth}\raggedright\strut
line rate\strut
\end{minipage} & \begin{minipage}[b]{0.06\columnwidth}\raggedright\strut
lanes\strut
\end{minipage} & \begin{minipage}[b]{0.07\columnwidth}\raggedright\strut
``size''\strut
\end{minipage} & \begin{minipage}[b]{0.14\columnwidth}\raggedright\strut
\texttt{\$f\_1,f\_2\$} DSB BW\strut
\end{minipage} & \begin{minipage}[b]{0.14\columnwidth}\raggedright\strut
BW mix 2nd+3rd\strut
\end{minipage}\tabularnewline

\endhead
\begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
	2.4GHz\strut
\end{minipage} & \begin{minipage}[t]{0.15\columnwidth}\raggedright\strut
600MHz\strut
\end{minipage} & \begin{minipage}[t]{0.09\columnwidth}\raggedright\strut
6GHz\strut
\end{minipage} & \begin{minipage}[t]{0.06\columnwidth}\raggedright\strut
8\strut
\end{minipage} & \begin{minipage}[t]{0.07\columnwidth}\raggedright\strut
4\strut
\end{minipage} & \begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
150MHz\strut
\end{minipage} & \begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
300--600--900MHz\strut
\end{minipage}\tabularnewline
\begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
	2GHz\strut
\end{minipage} & \begin{minipage}[t]{0.15\columnwidth}\raggedright\strut
1000MHz\strut
\end{minipage} & \begin{minipage}[t]{0.09\columnwidth}\raggedright\strut
10GHz\strut
\end{minipage} & \begin{minipage}[t]{0.06\columnwidth}\raggedright\strut
8\strut
\end{minipage} & \begin{minipage}[t]{0.07\columnwidth}\raggedright\strut
8\strut
\end{minipage} & \begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
125MHz\strut
\end{minipage} & \begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
500--1000--1500MHz\strut
\end{minipage}\tabularnewline
\begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
	1.6GHz\strut
\end{minipage} & \begin{minipage}[t]{0.15\columnwidth}\raggedright\strut
800MHz\strut
\end{minipage} & \begin{minipage}[t]{0.09\columnwidth}\raggedright\strut
8GHz\strut
\end{minipage} & \begin{minipage}[t]{0.06\columnwidth}\raggedright\strut
8\strut
\end{minipage} & \begin{minipage}[t]{0.07\columnwidth}\raggedright\strut
4\strut
\end{minipage} & \begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
200MHz\strut
\end{minipage} & \begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
400--800--1200MHz\strut
\end{minipage}\tabularnewline
\begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
	300MHz\strut
\end{minipage} & \begin{minipage}[t]{0.15\columnwidth}\raggedright\strut
300MHz\strut
\end{minipage} & \begin{minipage}[t]{0.09\columnwidth}\raggedright\strut
6GHz\strut
\end{minipage} & \begin{minipage}[t]{0.06\columnwidth}\raggedright\strut
4\strut
\end{minipage} & \begin{minipage}[t]{0.07\columnwidth}\raggedright\strut
2\strut
\end{minipage} & \begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
150MHz\strut
\end{minipage} & \begin{minipage}[t]{0.14\columnwidth}\raggedright\strut
150--300--450MHz\strut
\end{minipage}\tabularnewline

\end{longtable}

For 4 JESD lanes, use DAC ``mix mode'' (switching up-conversion by
\texttt{\$f\_\textbackslash{}mathrm\{DAC\}\$}) to emphasize second
Nyquist zone from \texttt{\$f\_\textbackslash{}mathrm\{DAC\}/2\$} to
\texttt{\$f\_\textbackslash{}mathrm\{DAC\}\$}. Zeros at 0Hz and
\texttt{\$2\textbackslash{}times\ f\_\textbackslash{}mathrm\{DAC\}\$}.
