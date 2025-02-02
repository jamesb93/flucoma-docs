:digest: Spectral Difference-Based Audio Buffer Slicer
:species: buffer-proc
:sc-categories: Libraries>FluidDecomposition
:sc-related: Guides/FluidCorpusManipulation
:see-also: OnsetSlice, BufAmpSlice, BufNoveltySlice, BufTransientSlice
:description: Implements a selection of spectrum-based onset slicers
:discussion:
   Performs segmentation based on the difference between spectral frames.
   
   The metric for calculating difference can be chosen from a curated selection, lending the algorithm toward slicing a broad range of musical materials.

   .. only_in:: sc

      The argument for ``metric`` can be passed as an integer (see table below), or as one of the following symbols: ``\power``, ``\hfc``, ``\flux``,	``\mkl``, ``\is``, ``\cosine``, ``\phase``, ``\wphase``, ``\complex``, or ``\rcomplex``. 

:process: This is the method that calls for the slicing to be calculated on a given source buffer.
:output: Nothing, as the various destination buffers are declared in the function call.

:control source:

   The buffer to use as the source material to be sliced through novelty identification. The different channels of multichannel buffers will be summed.

:control startFrame:

   Where in the srcBuf should the slicing process start, in samples.

:control numFrames:

   How many frames should be processed.

:control startChan:

   For multichannel sources, which channel should be processed.

:control numChans:

   For multichannel sources, how many channels should be summed.

:control indices:

   The buffer where the indices (in samples) of the estimated starting points of slices will be written. The first and last points are always the boundary points of the analysis.

:control metric:

   The metric used to derive a difference curve between spectral frames. It can be any of the following:

   :enum:

      :0:
         **Energy** thresholds on (sum of squares of magnitudes / nBins) (like Onsets \power)

      :1:
         **HFC** thresholds on (sum of (squared magnitudes * binNum) / nBins)

      :2:
         **SpectralFlux** thresholds on (difference in magnitude between consecutive frames, half rectified)

      :3:
         **MKL** thresholds on (sum of log of magnitude ratio per bin) (or equivalently, sum of difference of the log magnitude per bin) (like Onsets mkl)

      :4:
         **IS** (WILL PROBABLY BE REMOVED) Itakura - Saito divergence (see literature)

      :5:
         **Cosine** thresholds on (cosine distance between comparison frames)

      :6:
         **PhaseDev** takes the past 2 frames, projects to the current, as anticipated if it was a steady state, then compute the sum of the differences, on which it thresholds (like Onsets \phase)

      :7:
         **WPhaseDev** same as PhaseDev, but weighted by the magnitude in order to remove chaos noise floor (like Onsets \wphase)

      :8:
         **ComplexDev** same as PhaseDev, but in the complex domain - the anticipated amp is considered steady, and the phase is projected, then a complex subtraction  is done with the actual present frame. The sum of magnitudes is used to threshold (like Onsets \complex)

      :9:
         **RComplexDev** same as above, but rectified (like Onsets \rcomplex)

:control threshold:

   The thresholding of a new slice. Value ranges are different for each metric, from 0 upwards.

:control minSliceLength:

   The minimum duration of a slice in number of hopSize.

:control filterSize:

   The size of a smoothing filter that is applied on the novelty curve. A larger filter size allows for cleaner cuts on very sharp changes.

:control frameDelta:

   For certain metrics (HFC, SpectralFlux, MKL, Cosine), the distance does not have to be computed between consecutive frames. By default (0) it is, otherwise this sets the distance between the comparison window in samples.

:control windowSize:

   The window size. As spectral differencing relies on spectral frames, we need to decide what precision we give it spectrally and temporally. For more information visit https://learn.flucoma.org/learn/fourier-transform/

:control hopSize:

   The window hop size. As spectral differencing relies on spectral frames, we need to move the window forward. It can be any size, but low overlap will create audible artefacts. The -1 default value will default to half of windowSize (overlap of 2).

:control fftSize:

   The inner FFT/IFFT size. It should be at least 4 samples long, at least the size of the window, and a power of 2. Making it larger allows an oversampling of the spectral precision. The -1 default value will default to windowSize.

:control maxFFTSize:

   How large can the FFT be, by allocating memory at instantiation time. This cannot be modulated.

:control action:

   A Function to be evaluated once the offline process has finished and indices instance variables have been updated on the client side. The function will be passed indices as an argument.

