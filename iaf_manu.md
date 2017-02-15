---
title: Towards a reliable, automated method of individual alpha frequency (IAF) quantification
author:
- Andrew W. Corcoran 
- Phillip M. Alday
- Matthias Schlesewsky
- Ina Bornkessel-Schlesewsky
output: html_document
bibliography: ~/Dropbox/libraryAC.bib
# csl: ~/Dropbox/apa-old-doi-prefix.csl --> can't get this (or other csl versions) to run
---
<!-- AC: so far as i can tell, the only way to comment is via html tags. is there a markdown specific alternative? -->
## 1 Introduction
Oscillatory activity is an inherent property of neurons and neuronal assemblies, and the timing of oscillatory dynamics is thought to encode information [e.g. @fries2005;@buzsaki2004].
Neuronal oscillations reflect fluctuations between states of high and low receptivity, such that communication between individual neurons and broader neuronal populations is optimised via the establishment of oscillatory coherence [@fries2005].
Complex cognitive tasks typically require coordination between distant brain regions and systems, thus requiring effective connectivity to be established within task-relevant neural networks at relatively short timescales [@fries2005 ?palva11].
Task-irrelevant and potentially interfering connections must concomitantly be inhibited, i.e. task-relevant neural networks are gated by inhibition [@jensen2010].
The alpha rhythm of the human EEG is thought be the primary carrier of this inhibitory function [@klimesch2007;@klimesch2012;@jensen2010;@jensen2012;@sadaghiani2016], with alpha synchronisation in task-irrelevant regions reflecting inhibition, and alpha desynchronisation in task-relevant regions reflecting release from inhibition [@pfurtscheller2003].
This account is gaining increasing acceptance over alternative accounts of the alpha rhythm such as the proposal that it reflects cognitive idling [@adrian1934;@pfurtscheller1996].

While the importance of the alpha rhythm for cognitive processing has been recognised since Hans Berger’s seminal work on the human EEG in the early 20th century [@berger1929; cf. @adrian1934], a more recent line of research has focused on the importance of inter-individual variability in resting alpha activity for cognitive processing [cf. @klimesch1999 for a review].
According to this body of literature, the frequency at which alpha-generating neuronal circuits predominantly oscillate (i.e. the individual alpha frequency; IAF) while one relaxes in a state of alert wakefulness predicts performance across a variety of perceptual [e.g., @cecere2015] and cognitive [e.g., @bornkessel2004;@clark2004] tasks.
The IAF, which varies between approximately 9.5 and 11.5 Hz in healthy young adults [@klimesch1999], is a trait-like characteristic of the human EEG [@grandy2013a], which shows high heritability [@lykken1974;@malone2014;@posthuma2001;@smit2006] and test-retest reliability, [@gasser1985test;@kondacs1999;@napflin2007] as well as being stable across cognitive training interventions [@grandy2013a].
Individuals with a low IAF process information more slowly [@klimesch1996b;@surwillo1961;@surwillo1963], possibly due to decreased efficiency in thalamo-cortical networks [@klimesch1997;@steriade1990].
They also show a reduced performance on memory tasks [@klimesch1999] and general intelligence measures (*g*) [@grandy2013] in comparison to their high-IAF counterparts. 
IAF decreases with age from young adulthood onwards [@chiang2011;@klimesch1999;@kopruner1984;@obrist1979], and the age-related slowing of the alpha rhythm thus accompanies the well-known decline of many cognitive abilities in older adulthood [e.g. @hedden2004;@salthouse2011].
Taken together, this evidence suggests that IAF constitutes a promising neurophysiological marker of certain fundamental properties of central nervous system functioning [@grandy2013;@grandy2013a].

<!-- IBS: not sure we need following paragraph -->IAF modulates early brain responses that accompany basic aspects of visual perception [@klimesch2004;@koch2008], while individual alpha power and alpha timing (entrainment) affect well-known attentional phenomena such as the attentional blink [@maclean2012;@zauner2012]. 
Moreover, IAF-informed brain stimulation has been shown to improve cognitive performance [@klimesch2006] and to shift the boundaries of integration windows for cross-modal perception [@cecere2015].
Accordingly, individual differences in alpha activity appear to influence the efficiency of attentional filtering [@klimesch2012].
This mechanism is especially important for increasing the signal-to-noise ratio when attentional demands increase, for example in cognitive processing scenarios requiring top-down control or prediction [@klimesch2011].
Adjusting frequency band analysis in relation to IAF has also been argued to enhance sensitivity to band-specific oscillatory dynamics [@citation], and has proved useful for dissociating distinct functional properties of alpha-band sub-regions [@citation].
Indeed, recent evidence [@vanalbada2013] supporting the idea that the canonical frequency bands display an approximately harmonic relationship to the dominant alpha rhythm [@klimesch2012; ?cf. @rennie/robinson in va] suggests that IAF may offer a valuable tool for sharpening the precision of frequency domain analysis across the entire spectrum.

In spite of its promise as a marker of apparently enduring, trait-like individual differences in cognitive functioning, and its potential utility for tuning frequency band analysis to empirical properties of the PSD, no consensus currently exists as to the optimal method of quantifying IAF.
This paper thus sets out to develop a rigorous, automated strategy for estimating two of the most widely reported indices of IAF in the literature; namely, peak alpha frequency and alpha frequency centre of gravity.

### 1.1 Peak alpha frequency
The classical method of estimating IAF relies on delineating the peak alpha frequency (PAF); a singular, prominent peak within that portion of the power spectral density (PSD) plot assumed to constitute the domain of the alpha-band frequency range.
This expression can be formalised in terms of the local (i.e. relative) maximum within the alpha band:<!-- AC: PA recommended switching back to the argmax formulation, which i have tried to render below. note that i have also included additional terms to better reflect the additional requirements of the PAF mentioned in the informal definition above (singularity & prominance or nontriviality) --> 

$$ PAF = \text{arg max}_{\substack{x\in A \subseteq PSD }} f(x) \iff \text{arg max}_{\substack{x\in A \subseteq PSD }} f(x) = \text{singleton} \land \text{max}_{\substack{x\in A \subseteq PSD }} f(x) > \phi , $$ <!-- AC: can't get the substack command to work with the text element -->

$$ \text{arg max}_{\substack{x\in A \subseteq PSD }} f(x) := \{ x \mid x  \in A \land \forall y \in A : f(y) \leq f(x) \} , $$

where $\text{arg max}$ returns the frequency bin (or subset of bins) $f(x)$ containing the maximal power value $\text{max } f(x)$ registered within that subset of frequency bins $A$ that constitute the alpha band, and where $PSD$ denotes the complete set of frequency bins resolved by the spectral analysis. 
Note that, for the output of $\text{arg max }$ to qualify as an estimate of PAF, it must return a single frequency bin $f(x)$ containing a power value exceeding $\phi$, where $\phi$ defines the minimal threshold differentiating a substantive spectral peak from background noise. 
The definition of both $A$ and $\phi$ parameters pose non-trivial problems, to which we shall return shortly.

PAF estimates are typically extracted from parieto-occipital EEG channels while the participant relaxes with their eyes closed.
This strategy exploits the classic observation that alpha oscillations dominate the EEG recorded over centro-posterior scalp regions when visual sensory input is suppressed [@barry2007;@sadaghiani2016]. 
Although PAF can in many cases be rapidly ascertained upon visual inspection of the PSD function<!-- refer to figure-->, this approach to IAF extraction is inefficient and potentially impractical when dealing with large datasets [@goljahani2012]. 
Moreover, it is well documented that a sizeable proportion of individuals fail to manifest an unambiguous PAF, either on account of there being more than one prominent peak within the alpha band [e.g., so-called ‘split-peaks’; @chiang2011], or due to the general lack of rhythmic alpha activity [e.g., @anokhin1996]. <!-- ? refer to figure e.g.s -->
Under the former circumstances, the adjudicator<!-- AC: this term has proved controversial. i deliberately wanted to underscore the tacit subjectivity involved in judging whether a given PSD manifests a (singular) peak. i'm open to alteratives but would like to retain this connotation, if possible. --> must decide whether a single, primary peak can be justifiably discerned amidst competing peak candidates; under the latter, they must decide whether the signal is too noisy to derive reliable inferences pertaining to IAF. 
Cases such as these may be prone to biased or inconsistent assessment, pose significant challenges to the replicability of analytic procedures, and can result in the exclusion of a substantial subset of participants from IAF-related analyses [see for e.g., @bornkessel-schlesewsky2015].

While simple automated peak detection methods are relatively straightforward to implement in many popular EEG processing software packages, such strategies are prone to various sources of error. 
For instance, searching for the maximal power estimate $\text{max } f(x)$ within the alpha window $A$ can result in the arbitrary assignment of PAF at $f_1$, the lower bound of $A$. 
That is, in the case where the PSD function declines approximately monotonically (e.g., conforms to the $1/f$ 'power law' distribution characteristic of the log-scaled PSD without showing the expected deviation about 10 Hz), the highest power value will be that which is encountered at the lower bound of the selected alpha band interval. 
One solution to this problem is to stipulate that $\text {arg max} f(x)$ qualifies as a viable PAF estimate if the power estimate of frequency bin $f(x)$ exceeds that of its neighbouring frequency bins $f(x_{-1})$ and $f(x_{+1})$. 
While this approach ensures that the selected frequency component constitutes a local peak within the alpha range of the PSD, it is still vulnerable to two of the problems identified above. 
First, it fails to distinguish spectra featuring singular, dominant peaks from those possessing prominent secondary peaks (i.e. where visual inspection of the plot would suggest two or more alpha peaks that differ in height by some – potentially trivial – magnitude). 
Second, it fails to differentiate maximum power values at the apex of a genuine spectral peak from those at the apex of noisy fluctuations within the PSD function (i.e. where visual inspection of the plot would suggest the absence of any substantive alpha peak). 
Automated routines of this sort might therefore render rapid and consistent estimates of PAF, but are too liberal to guarantee convergence with those derived from visual analysis of the PSD.
<!-- PA (lines 60-66): Not everything “follows” in a strict sense – the “solution to a problem” doesn’t really address a “problem” per se. Also, do you care about deviations from the 1/f power distribution (relative power) or absolute power? AC response: I still don't get this objection. Pragmatically (if not formally), the literature I've read tends to treat the assignment of f1 as PAF inappropriate if the bin preceding f1 would have likewise been assigned the PAF if that bin had been set as the search window boundary. [some] simple peak finding functions get around this in precisely the way i've described, i.e. returning a frequency bin i as the index of a peak iff both bin i+1 and bin i-1 contain ordinates that are < that of bin i, even if bin i-1 is not contained within the frequency window stipulated as the searched domain. in regards to your 2nd point, i really haven't given this much thought. some authors discuss peak estimation in the context of log transformed data, others the 'raw' PSD estimate, but they don't tend to justify one approach over the other (klimesch may mention somewhere that the log data makes the f1-f2 bounds easier to see). to the best of my knowledge, no one goes into the finer details of the distinction you draw (although some of the modeling stuff talks about deviation from the power-law when assessing whether a peak is a peak or simply a trivial fluctuation of background noise, in much the same way as my minP threshold works). this perhaps needs more careful consideration but there does not seem to be much to go off in terms of previous conventions/practices, etc. -->

### 1.2 Alpha centre of gravity and individualised frequency band intervals
Klimesch and colleagues [@klimesch1993; @klimesch1997] proposed using alpha centre of gravity frequency (CoG), an estimator of IAF originally formulated by Klimesch, Schimke, Ladurner, and Pfurtscheller [@klimesch1990], in order to circumvent some of the difficulties posed by the absence of an obvious spectral peak. 
CoG furnishes a weighted sum of spectral estimates divided by total power within the selected alpha frequency window $A$, defined by the bounds $f_1$ and $f_2$:

$$ CoG = \frac { \sum\limits_{f_1}^{f_2} a(f(x)) \times f(x) } { \sum\limits_{f_1}^{f_2} a(f(x)) }, $$

where $a$ is the PSD estimate (ordinate) for frequency bin $f(x)$.

Since the CoG is sensitive to the shape of the power distribution within the selected alpha band window, and the precise bandwidth of alpha-rhythm activity varies across individuals, Klimesch and colleagues [@klimesch1990, see also @klimesch1997] discouraged calculating the CoG according to a fixed index of summation corresponding to some standard, a priori-defined alpha bandwidth (e.g., $f_1$ = 8 Hz, $f_2$ = 13 Hz). 
Rather, they recommended computing CoG on the basis of bespoke frequency windows that capture the entire range of the individual’s alpha-band activity. 
To this end, Klimesch and colleagues [@klimesch1990] proposed the following procedure for estimating the IAF bandwidth: 
First, PSD plots are extracted from all EEG channels for each participant and examined for evidence of a clear alpha peak. 
Second, $f_1$ and $f_2$ are assigned to those frequency bins where the ascending and descending edges of the peak are deemed to start and end, respectively [for an e.g., see @klimesch1997, p. 322]. 
Finally, these channel-wise $f_1$ and $f_2$ values are averaged to render the bounds of the frequency interval that will be used to calculate the participant’s CoG. 
Notice that, even though EEG channels that fail to manifest distinctive peaks do not contribute to the definition of the IAF bandwidth, the CoG is computed on the basis of spectral data compiled from $f_1 – f_2$<!-- AC: is it better to use colon notation here ? or elipses ? --> across all available channels.

Klimesch [@klimesch1999] later proposed an alternative method for defining individualised alpha-band windows that relies on a somewhat less subjective interpretation of the PSD. 
This technique exploits the typically observed anticorrelation between theta band (~4–8 Hz) and alpha oscillatory dynamics in response to task demands or cognitive load [@doppelmayr1998a; @klimesch1996a; cf. @rugg1982]. 
Klimesch and colleagues [@klimesch1996a] first used this approach to delineate the ‘transition frequency’ (TF) where the alpha-band activity that dominates the relaxed individual’s EEG gives way to theta oscillations induced by stimulus processing. 
Adopting an event-related desynchronisation [ERD; @pfurtscheller1977; @pfurtscheller1999] paradigm, spectra from a pre-stimulus ‘reference’ interval and a peri-stimulus ‘test’ interval were averaged across trials and superimposed, and the TF designated as the frequency at which the PSD functions derived from the reference and test intervals intersect (or where the difference between them is minimised, in cases where they fail to intersect). 
Klimesch [@klimesch1999] proposed generalising this procedure to resting-state EEG recordings, where an analogous shift from prominent alpha- to theta-band activity is classically evoked by the visual stimulation induced upon opening the eyes [this phenomenon, known variously as alpha blocking, desynchronisation, suppression, or attenuation, was first documented by @berger1929]. 
This method thus renders a systematic means of estimating the lower alpha frequency bound $f_1$. 

In the absence of any analogous means of inferring the upper bound of the alpha-band window, Klimesch [@klimesch1999] recommended determining $f_2$ on the basis of $f_1$ and the IAF.
One suggestion was to set $f_2$ rather pragmatically at 1-2 Hz above the IAF, or alternatively to subtract the difference between IAF and $f_1$ (i.e. the lower alpha band) from some presupposed alpha bandwidth (e.g., if the alpha interval is assumed to span 5 Hz, and the individual manifests a TF of 7 Hz and an IAF of 10.5 Hz, $f_2 = 5 - (10.5 - 7) + 10.5 = 12$ Hz). 
Although these approaches do indeed adjust the value of $f_2$ in relation to the IAF, they are insensitive to interindividual differences in alpha bandwidth [@doppelmayr1998; @goljahani2012]. 
A more promising solution that attempts to capture such variance is given by calculating $f_2$ as a proportion of the difference between IAF and $f_1$ [@doppelmayr1998;@klimesch1999]; e.g., $f_2 = ( (IAF - f_1) / 2 ) + IAF$. While this heuristic offers a more nuanced approach towards individually adapted estimates of $f_2$, it clearly depends on the assumption that the proportional difference between IAF and alpha bandwidth is consistent across individuals (such that the lower alpha band spans approximately double the frequency range of the upper alpha band).
Furthermore, all of these methods presuppose that IAF has already been estimated via the PAF (since CoG cannot be calculated prior to the definition of $f_1$ and $f_2$). 
This is obviously problematic given that one of the chief advantages of the CoG is its supposed capacity to deliver IAF estimates when the PAF is unavailable.

### 1.3 Peak attenuation and channel reactivity based (CRB) methods
We turn now to two interesting extensions of the TF approach that depend on the alpha blocking or desynchronisation phenomenon mentioned above. 
The first method, which we refer to as peak attenuation, was described by @posthuma2001.
Similar in principle to Klimesch's [@klimesch1999]<!-- AC: check how to be more flexible with citation format --> delineation of the TF, this technique simply subtracts eyes-oped PSD estimates from the corresponding ordinates of the eyes-closed PSD within some a priori defined alpha-band window (e.g., 7 – 14 Hz).
The resulting peak, which constitutes the maximal power difference between the two spectra, is taken as the PAF. 
To avoid trivial peaks arising from small fluctuations in the PSD, Posthuma and colleagues excluded spectra featuring consistently low power estimates (< 1.5 $\mu$V/Hz) within the alpha-band. 
It isn’t clear, however, whether this criterion (which is analagous to the $\phi$ parameter in the PAF equation defined above) was also applied in cases where the maximal difference between spectra was similarly small (i.e. where both spectra present a substantive, near overlapping alpha-band component).
Posthuma and colleagues also remarked that peak attenuation estimates deviated from eyes-closed resting-state PAFs in 21% of cases. 
This lack of convergence, coupled with the observation that peak attenuation may lead to distorted representations of the PAF when the assumption of significant eyes-open alpha desynchronisation fails to obtain<!-- AC: refer to fig where peak est shifted, ? refer to ind diffs in EO/EC alpha-->, suggests peak attenuation may constitute a suboptimal estimator of IAF (at least insofar as IAF is conceived in terms of the PAF).

Although Posthuma and colleagues [@posthuma2001] did not attempt to locate the bounds of the individual alpha bandwidth on the basis of peak attenuation, the logic motivating this technique would suggest that $f_1$ and $f_2$ could be inferred in much the same way as the TF (i.e. by taking those frequency bins either side of the maximal power difference peak in which the difference between corresponding spectral estimates is minimised).
This is precisely the approach that Goljahani and colleagues formalised in their channel reactivity based (CRB) method [@goljahani2012; @goljahani2014]. 
This technique, which is conceptually reminiscent of Klimesch and colleagues’ [@klimesch1996a] attempt to characterise phasic shifts in band power, quantifies the difference between reference and test PSDs in terms of the alpha responsiveness (or desynchronisation) region; i.e. the area between the PSD functions spanning frequency bins $f_1 - f_2$ (where the spectra intersect, or the residual difference between corresponding spectral estimates is minimised). 
IAF is estimated by computing the CoG for the reference interval, taking the frequency bounds delimiting the responsiveness region as the index of summation.

The CRB method offers an elegant solution to the problem of finding individualised frequency bands for CoG estimation. 
Not only does it improve on previous attempts to characterise the bounds of the alpha range by means of a rapid, automated, and empirically driven appraisal of reference vs. test interval PSDs, it does so without any assumptions about (or dependence on) the peak-like quality of the spectra. 
As such, CRB promises to maximise the potential utility of the CoG as an estimator of IAF that can be reliably computed irrespective of the presence or absence of a single, distinct peak component. 
We note however that, much like the peak attenuation technique, CRB estimates may be prone to distortion in cases where the ERD task elicits asymmetrical patterns of alpha-band desynchronisation [see @klimesch1997, for evidence of the dissociation between lower and upper alpha-band reactivity]. 
This is because the partial overlap of reference and test spectra that will ensue should the test stimulus fail to evoke comprehensive alpha desynchronisation will restrict the responsiveness region to a limited segment of the alpha band, thus precipitating a narrower (and potentially shifted) index of summation. 
Indeed, this phenomenon might go some way to explaining the substantial discrepancies observed between some of the CRB CoGs reported by Goljahani and colleagues (some of which were extreme by conventional standards; e.g., 14.9 Hz) and the corresponding PAFs derived from the same set of channel data [@goljahani2012].<!-- AC: e.g. fig 6(d), apparently trivial reactivity window, mostly located in beta range, Cog ~17 Hz. ?NaN preferable -->

A related concern deriving from the CRB method’s reliance on phasic changes in rhythmic activity is the possibility that within-subject estimates of IAF might vary depending on the specific processing mode evoked by the event (e.g., target discrimination vs. memory retrieval; visual vs. auditory modality), and the relative timing of the reference/test intervals subjected to spectral analysis. <!-- PA: How do these concerns interact with the idea that IAF is trait-like? AC response: Several authors posit that there are a variety of alpha generators, the cumulative activity of which accounts for the characteristic resting peak (or peaks), and thus shifts in peak frequency mark shifts in the relative activity of these populations. The argument here then is that, while resting eyes closed IAF is stable/enduring and indicative of at least some cognitive capacities (presumably by tapping into specific underlying neuronal processes), measures of alpha activity during cognitive tasks (or the difference in alpha activity between different states of rest/anticipation/action) may not offer an analagous index of IAF as construed by research involving EC resting state data. I see no necessary contradiction between the notion that resting state IAF is stable/'trait-like' etc, and the observation that dominant alpha band activity dynamically shifts during 'non-restful' states (presumably as a consequency of the shifting division of labour across alpha generators). indeed, since you asked me this question, i've re-read a few articles and noticed a number of references to this idea of IAF as possessing both trait- and state-like qualities-->
ERD studies have revealed that both the qualitative profile and temporal course of alpha- and theta-band desynchronisation are contingent upon the particular nature of the task used to induce oscillatory power shifts [@klimesch2006; @klimesch2007]. 
If different paradigms do evoke distinct patterns of ERD [or even increase the synchronisation of the alpha rhythm; see for e.g., @rihs2007] during the selected test interval, then the ensuing responsiveness regions used to define the coverage of the CoG estimate will span non-identical frequency bands [cf. @haegens2014, for evidence of analogous intraindividual shifts in PAF in response to varying task conditions]. 
While this property of the CRB method need not be a problem for ERD-type applications (indeed, sensitivity to such selective changes in band power might prove theoretically interesting and productive in this context), they are suboptimal when attempting to evaluate the IAF as a stable, trait-like marker of general cognitive processing capacities.  

### 1.4 Parametric curve-fitting approaches to alpha rhythm quantification
Finally, we turn briefly to a promising line of research that attempts to quantify the spectral features of EEG data, and in particular spectral peaks, by means of statistical curve-fitting techniques. 
Chiang and colleagues [@chiang2008] developed an algorithm (with corresponding implementation in C) that parameterises alpha-band peaks via a two-step procedure: Peaks are first identified and parameterised via the fitting of a Gaussian function, before being fine-tuned in relation to the spread of fitted estimates across multiple electrode sites. 
Chiang and colleagues [@chiang2011] and van Albada and Robinson [@vanalbada2013] later demonstrated the potential utility of such automated routines by applying this general technique to a dataset comprising over 1400 individuals. <!-- AC: i haven't got to the end of this para yet -->

Given the manifest advantages of these analysis tools, it is somewhat puzzling that they do not yet appear to have gained widespread currency in the contemporary IAF literature [for instance, neither @goljahani2012, nor @bazanova2014, mention the development of such algorithms in their reviews of IAF methods; although cf. @haegens2014, for a notable counterexample]. 
One possibility is that researchers are simply unaware of the existence of these methods, since the bulk of the literature in which they have been applied is concerned with broader questions of spectral modeling, rather than the quantification of IAF per se [@haegens2014's investigation of PAF variability again the exception). 
An alternative explanation is that researchers deem these methods too complex to be a worthwhile investment of their time (especially if quantifying IAF is only an intermediary step towards further frequency domain analysis, rather than the main focus of the enterprise). 
This attitude may be reinforced by the additional burden involved in obtaining and implementing an algorithm that may have been written in an unfamiliar programming language, and that may pose nontrivial challenges pertaining to its integration within existing analysis pipelines. 
We suggest then that one of the critical steps towards achieving a more widespread application of such automated IAF estimation routines is to make these tools as openly available as possible, in formats that are easy to assimilate within popular methods of EEG analysis.

### 1.5 Aims of the present study
In sum, common methodological approaches to IAF estimation are either (1) time-consuming and vulnerable to inconsistencies arising from qualitative interpretation, (2) at risk of producing spurious or biased estimates under certain plausible spectral conditions, (3) conflate trait-like alpha properties with variable phasic effects, or (4) show some combination of the above. 
More recent innovations designed to address these problems via the application of sophisticated curve-fitting algorithms have so far found limited uptake within the broader IAF literature, perhaps on account of practical barriers pertaining to software access and implementation.<!-- AC: PA suggested citing something here about open source / open science, but I'm struggling to find a good paper. Would a blog post from a reputable source be ok ? -->
Consequently, we seek to articulate an automated method of alpha-band quantification that provides fast, reliable, and easily replicated estimates of resting-state IAF in conjunction with EEGLAB [@delorme2004], a popular open source EEG data analysis toolbox.

Our approach aims to emulate Klimesch and colleagues’ [@klimesch1990] original attempt to characterise individual profiles of resting-state oscillatory activity across the entirety of the alpha band by means of a relatively simple, non-parametric curve-fitting algorithm. 
Our strategy for accomplishing this task runs as follows: <!-- AC: am I correct in thinking it's non-parametric because it makes no assumptions about the underlying shape of the function (only that it can be fitted by polynomials) ? -->
First, we extract PSD estimates from preprocessed and fast Fourier transformed EEG signals. 
Second, we apply a least-squares curve-fitting procedure (i.e. Savitzky-Golay filtering) to accomplish the dual task of smoothing the PSD function and estimating its first- and second-order derivatives.
Third, these derivative functions are analysed to evaluate the quality of evidence that a distinct spectral peak (i.e. PAF) exists within the alpha frequency region.
Finally, the first derivative of the PSD is reanalysed to delineate upper and lower bounds of the individualised alpha interval, which are taken as the index of summation required to calculate the CoG. 
The effectiveness of this routine will first be demonstrated using human (i.e. non-simulated) EEG data. 
We will then turn to simulated data in order to assess how well our proposed technique performs under conditions of varied spectral composition and signal-to-noise ratio (SNR).

## 2 Method

### 2.1 Overview of methodological approach: Differentiation as a means of spectral peak quantification
In the following section, we intend to show how differential calculus can be profitably exploited for the purposes of alpha peak parameterisation. 
First, we outline how spectral peaks (and troughs) can be localised via differentiation of the first-order derivative. 
We then address the problem of multiple (potentially trivial) zero crossings, and propose Savitzky-Golay filtering as an elegant solution to this concern. 
Finally, we turn to the second-order derivative in order to arrive at a means of evaluating the quality (or ‘prototypicality’) of individual channel peak estimates.

#### 2.1.1 Local extrema and first derivative zero crossings 
<!-- AC: PA suggested deleting / relegating this section to the supp mats, as will be rather elementary for a proportion of readers. At the moment I'm inclined to keep it unless the reviewers kick up a fuss. When PA asked me to consider my target audience, I think that would be me ~6 months ago (and I would have needed this section to make sense of the method). I'm open to persuasion on this point though. -->As pointed out by Grandy and colleagues [@grandy2013; @grandy2013a], one solution to the problem of automated peak detection is to search for downward going zero crossings in the first derivative of the PSD. 
Derivatives describe the relative rate of change in the dependent variable or function $y$ given some value of independent variable $x$. 
The first derivative of a vector of PSD estimates thus provides point estimates of the (instantaneous) rate of change in the amount of spectral power estimated for each frequency bin resolved in the analysis. 
This relationship can be formalised as follows:

$$ f’(x) = \frac{\Delta y} {\Delta x} , $$

where $f’(x)$ is the first derivative of the relative change in the power estimate $y$ at frequency bin $x$.

Another way to conceptualise this relationship is to construe the derivative as describing the slope of the tangent line to the PSD function $y$ at any given frequency bin $x$. 
From this perspective, it becomes clear that the first derivative will be zero (i.e. the slope of the tangent will be horizontal) at any point in the function corresponding to a peak or trough. 
In the case of the former, the derivative will change from a positive value (as the function ascends towards its peak) to a negative value (once the function begins to descend) as the tangent traverses the local maximum. 
As such, positive to negative sign changes (i.e. downward going zero crossings) within the first derivative offer a convenient index of local maxima. 
Conversely, sign changes in the opposite direction (i.e. upward going zero crossings) can likewise be used to identify local minima.

#### 2.1.2 Savitzky-Golay smoothing and differentiation
Although Grandy and colleagues [@grandy2013; @grandy2013a] correctly assert that downward going zero crossings avoid the problem of arbitrary boundary effects in cases such as that of the (approximately) monotonic PSD<!-- PA: When the curvature is zero, a local optimum on a boundary would still be a local optimum even if the boundary weren’t there. AC response: I recall having some disagreement with PA about my terminology here. I'm not sure I fully understand the criticism, or if i've been misleading in my description of the method. Do you mean in the case of a constant function, where there is no change in y as a function of x ? If so, the zero crossing technique can indeed handle this situation, as it looks for changes from a positive to a negative derivative value (or vice-versa) rather than a zero value per se. -->, they fail to articulate a systematic method for differentiating substantive peaks from noisy spectral fluctuations. 
We suggest that the situation in which spectral analysis is degraded by signal noise can be substantially improved via the application of a smoothing procedure. 
The idea here is to attenuate such noisy fluctuations about the hypothesised true alpha peak so that the vast majority of zero crossings derived from these trivial variations are eliminated from the signal. 
However, since standard filtering techniques (such as the moving average) may result in markedly distorted representations of the underlying peak structure [e.g., @zeigler1981], the challenge is to find a smoothing operation that preserves the spectral characteristics at stake in IAF analysis.

With this concern in mind, we turn to the Savitzky-Golay filter (SGF), a least-squares curve-fitting procedure specifically designed to aid in the detection of spectral peaks amidst noisy conditions [@savitzky1964]. 
The SGF has a number of properties that make it well suited to the task of smoothing PSD functions, not least of which is its capacity to render smoothed signal curves that conserve the height, width, position, area, and centre of gravity of the underlying component structure [see @ziegler1981].
SGFs work by centring a sampling window of length $F_w$ on a portion of the input signal and fitting a polynomial function to each $i$^th^ data point spanned by $F_w$. 
The window is then shifted one point along the signal for the next polynomial fit computation. 
The smoothed function generated by the SGF operation constitutes the centre value output by each polynomial fit. 
For recent technical treatment of the SGF and its performance properties, the interested reader is referred to Schafer [@schafer2011].

In addition to its smoothing capability, SGFs can also be applied to calculate the $n$^th^ order derivative of the input signal. 
This is of course particularly convenient in the present context. 
Indeed, commensurate with their desirable spectral smoothing characteristics, SGFs have likewise been shown to manifest optimal (or near optimal) performance as digital differentiators [@luo2005]. 
We thus put forward the SGF as a valuable tool for both (a) refining the precision of standard methods used to characterise the spectral profile of alpha-band rhythms, and (b) improving the reliability of first derivative zero crossing approaches to peak (and trough) localisation. 
Before describing how the dual function of the SGF can be implemented in IAF analysis, however, we turn to one final innovation involving the second derivative.

#### 2.1.3 Assessment of peak quality
Typically, resting-state EEG recordings afford data from multiple electrode channels, a selection of which may contribute to the calculation of the IAF estimate. 
Channels that are in close proximity to one another are expected to produce highly correlated data; hence, a set of channels concentrated on the centro-posterior region of the scalp should ideally render highly convergent estimates of spectral power. 
However, since channels may be differentially affected by various sources of signal noise (e.g., high or fluctuating levels of impedance between scalp and electrode), SNR might be degraded in analyses that treat all data sources uniformly.
We therefore propose an automated method of peak analysis that seeks to evaluate which channels provide the strongest evidence of a prominent alpha peak, such that these channels will be assigned heavier weights when deriving the average PAF across channels. 
This procedure is intended to sharpen the precision of PAF estimation amidst cross-channel PSD variability without resorting to the exclusion of channels that are less conformant to the conceptual ideal of a high powered, narrowly defined spectral peak.

This approach relies on differentiation of the second derivative of the PSD function:

$$ f’’(x) = \frac {\Delta f(x)’} {\Delta x’} , $$ <!-- AC: do i need the prime on the denominator here?-->

where $f’’(x)$ is the derivative of the first derivative $f’(x)$ at frequency bin $x’$.
In other words, the second derivative is simply the rate of change of the first derivative of some function $f(x)$. 
Second derivatives are useful for evaluating whether the curvature of a function is concave up (i.e. convex) or concave down at any given value of $x$. The transition of a curve’s direction between concave up and concave down is characterised by an inflection point, which registers a second derivative value of zero.

We suggest that the inflection points $i_1$ and $i_2$ on either side of $\text{max } f(x)$ offer a convenient objective standard for evaluating the relative quality of channel peaks. 
The basic idea here is to quantify the area under the peak in such a way that distinguishes stronger (i.e. containing a greater proportion of spectral power) and less variable (i.e. spanning fewer frequency bins) peaks from shallower, broader, or otherwise noisier peaks. 
Given a set of spectral data from a variety of electrode channels, those PSDs which give rise to higher quality peaks (as operationalised above) are weighted more heavily than their less prominent counterparts, and thus contribute relatively more information to the calculation of the cross-channel PAF estimate. 
Note that this procedure has no bearing on CoG estimation (since the CoG may be derived from spectra in which no clear evidence of a single alpha peak was detected, and thus for which no inflection point data are available).

We claim $i_1$ and $i_2$ constitute a valuable marker of peak width across a variety of spectral conditions. 
In cases where the shape of the alpha peak is sharp and narrow, $i_1$ and $i_2$ are expected to be located towards the tails of the individual alpha band, and indeed could be construed as somewhat analogous to a conservative approximation of $f_1$ and $f_2$, respectively. 
However, in cases where secondary peaks are apparent, $i_1$ and $i_2$ will ensure that the peak evaluation procedure is constrained to that region underneath the primary peak. 
This latter scenario helps to highlight the advantage of an approach reliant upon inflection points as opposed to common alternative measures of spectral width, such as the base width or full width at half maximum (FWHM). 
These latter techniques are ill-equipped to distinguish the bounds of a primary peak amidst a broader spectral region comprising multiple component structures. 
Such measures may therefore be prone to rendering excessively liberal estimates of peak area.

Having defined both the height and width of the putative alpha peak by means of the first and second derivative zero crossings, peak quality is quantified via the following formula:<!-- AC: I've used integration here, or more properly an approximation to integration rendered via MATLABs `trapz` function, but I wonder if it's better to use the sum of frequency bins approach as per the CoG formula in order to draw out the similarities with the CoG calculation ? -->

$$ Q = \frac{\int_{i_1}^{i_2} a(f(x)) } { i_2 – i_1 } , $$
<!-- PA: From the Mean Value Theorem, this is exactly the “mean” power on that interval, which tells you a bit about how strong the peak is – peaks have higher mean values -->
where $Q$ is the scaled quantity of normalised alpha peak power, and $a$ is the estimated power at each frequency bin $f(x)$ that falls within the index of integration $i_1 - i_2$. 
Note that the inclusion of the denominator ensures that spectral width is taken into account when calculating $Q$.  
Given equal values of $\int_{i_1}^{i_2}f(x)$, the denominator adjusts the integrand such that narrower, sharper peaks are assigned a larger $Q$ value than their broader, flatter counterparts. 
This formulation thus penalises ‘less peaky’ components by assigning a heavier weighting to what we claim constitute stronger evidence of the individual PAF. 
However, it is perhaps worth emphasising that this calculation only influences PAF estimation in cases where channel data produce divergent peak frequencies (i.e. relative $Q$ weights have no impact on the mean PAF calculated from channels that furnish identical estimates of the peak frequency).<!--AC: definitely need an example figure here to show what Q weighting does -->

### 2.2 Implementation of proposed analysis techniques

#### 2.2.1 Software requirements
The afore-described approach to IAF estimation has been implemented via a set of customised functions programmed in MATLAB^&reg;^ (The MathWorks, Inc., Natick, MA, USA). 
Note that these routines are dependent upon the Signal Processing Toolbox&trade; and the EEGLAB toolbox.
EEGLAB is necessary for data importation, since our analysis programme assumes that EEG data are structured according to EEGLAB conventions. 
Signal Processing Toolbox is required for the `pwelch` and `sgolay` functions, which are responsible for executing the PSD estimation and SGF design components of the programme, respectively. 
`sgolay` was preferred over `sgolayfilt` on the basis that it outputs the coefficients necessary for calculating higher-order derivative functions (in addition to those for the zero-order, i.e. smoothed, PSD function). 
All functions developed to conduct the following analysis are open source and can be accessed (along with sample datasets) via [GitHub](https://github.com/corcorana/restingIAF).

We chose to base our IAF estimation strategy on a Hamming-windowed implementation of Welch’s method of PSD estimation [@welch1967] on account of the prevalent application of this technique in the IAF literature. 
It should be noted however that our approach could quite readily be modified such that an alternative method of PSD estimation were substituted for the `pwelch` routine. 
We also stress that our selection of the Hamming window to taper the data prior to FFT does not reflect any strong commitments concerning the optimal method of preparing EEG data for spectral analysis: it stands rather as one of several reasonable options that could have been applied. 
Indeed, while a rigorous comparison of how our IAF estimation technique performs given various methods of extracting PSD data would be desirable, it is beyond the scope of this paper. 
Our focus here, rather, is to provide a convincing proof of concept for the general tenets of our smoothing-differentiation approach.

#### 2.2.2 Parameters for estimating PAF and CoG
A number of parameters must be specified in order to execute the IAF estimation programme. 
In relation to the SGF, both polynomial degree $k$ and a filter window frame width $F_w$ are required to define the least-squares minimisation operation.
$k$ must be $< F_w$, and $F_w$ must be odd to ensure an equal number of sample points either side of the centre coefficient. 
Also note that no smoothing will occur if $k = F_w - 1$. 
Although some degree of exploratory analysis may be desirable to find the optimal combination of $k$ and $F_w$ parameters for the dataset under analysis, a convenient heuristic is to set the length of $F_w$ approximately 1 to 2 times the anticipated FWHM of the PAF component [@enke1976; @press1992]. <!-- IBS: Doesn’t this detract somewhat from the ‘objectivity’ and replicability of the approach? Can we provide a more precise default choice (or an algorithm for finding one) to ensure that anyone using the procedure with this standard parameter choice will obtain the same results? The same applies to all other parameters needing to be set… but perhaps section 2.2.4 will clarify this. AC response: I completely agree this seems like a concession that undercuts some of the previous arguments. The 'cost' of using the SGF is that it comes with this flexibility in choice of Fw and k, the setting of which (general heuristics aside - which are only of limited use assuming peak width varies across individuals) can look a bit arbitrary. I also think some degree of exploratory fitting is justified if trying to extend the method out to populations that may not be expected to show the sort of classical peak components we associate with healthy young adults (older adult/clinical/children etc). Of course, so long as param settings are known, there is no problem on the replication side of things. I guess the only worry is people might fish around to get the peaks they want / exclude the ones they don't -->

If smoothed peaks seem excessively flat, $F_w$ is likely to be suboptimally broad. 
We favour higher-order polynomials (e.g., $k = 5$) due to their peak-height preserving properties, but acknowledge they may render suboptimal fits in the context of relatively broad component structures. This may be of particular concern for instance when dealing with elderly populations, given the increased likelihood of diminished peak density in older adults [@chiang2011].<!-- AC: has anyone got a good citation to the effect that older people often have less powerful/broader alpha peaks ? i thought i had, but can't seem to find it --> 

Additional parameters include:
$W_\alpha$, the domain of the PSD corresponding to the putative alpha bandwidth;
$minP$, the minimum quantity of normalised power required to qualify as a potential PAF candidate (fixed automatically according to the statistical properties of the spectrum);
$pDiff$, the minimum proportion of peak height by which the highest peak component must exceed all competitors in order to qualify as the PAF; 
and $cMin$, the minimum number of channels necessary for the computation of cross-channel IAF estimates. 
Examples of what we consider to be reasonable parameter values are outlined in section 2.3.4.

#### 2.2.3 Overview of analysis procedure
The analysis pipeline can be summarised as follows<!-- AC: ?include a diagram of the procedural flow-->: Once parameters have been defined and preprocessed data have been imported into the workspace, the PSD is estimated for each channel included in the analysis. 
These data are then normalised via the division of each ordinate by the mean spectral power estimated for each individual channel spectrum (where filtering has been applied during preprocessing, normalisation is applied after the PSD has been truncated such that only those frequency bins bounded by the filter passband are included). 
Channel-wise PSDs are then subjected to the SGF in order to estimate their zeroth (i.e. smoothed), first, and second derivatives. 
Next, the first derivative is searched for downward going zero crossings within $W_\alpha$. 
If no zero crossings are identified, the channel is excluded from further analysis. 

Identified zero crossings are first assessed as to whether they satisfy $minP$, which is calculated by fitting a least-squares regression line to the normalised power spectrum. 
The ordinate at the zero crossing must exceed the power value predicted by the regression fit in order to register as a potential candidate for the PAF. 
This parameter thus provides a convenient threshold for the exclusion of zero crossings emanating from trivial fluctuations in the power spectrum<!--see fig contrasting 2 e.g.s-->. 
If more than one peak within the spectral domain defined by $W_\alpha$ is found to exceed $minP$, these candidates are rank ordered according to their normalised power estimates, and the magnitude difference between the two largest peaks is compared. 
If the primary peak exceeds the height of its closest competitor by more than the threshold defined by $pDiff$, it is assigned as the PAF. 
The second derivative is then examined to determine the location of the associated peak inflection points, and the $Q$ value is subsequently computed. 
Should the primary peak fail to satisfy the $pDiff$ criterion, this channel would fail to register an estimate of PAF. 
It would however still qualify for inclusion in the CoG estimation procedure.

CoG calculation follows the standard procedure described by Klimesch and colleagues [@klimesch1990], with the exception that the bounds of the alpha interval were automatically detected. 
The programme derives these bounds by taking the left- and right-most peaks within $W_\alpha$ (i.e. those peaks in the lowest and highest frequency bins, respectively; these may coincide with the PAF), and searching the first derivative for evidence of the nearest local minimum prior to and following these peaks, respectively.
Since some spectra show a relatively shallow roll-off away from the alpha peak, one that does not culminate in a local minimum for several Hz, we relaxed the requirement for an upward going zero crossing (i.e. evidence of a local minimum) such that the transition into a prolonged shallow function is taken as sufficient evidence of the individual alpha bounds $f_1$ or $f_2$.
This criterion was formalised as $f’(x) < |1| \text{ for } f(x_{k-1}...x_k) \text{ for } f1$, and $f’(x) < |1| \text{ for } f(x_k...x_{k+1}) \text{ for } f_2$, where $f(x_k)$ is the first encountered frequency bin where $f’(x) < -1$, and $k\pm1$ equates to the frequency bin 1 Hz above/below $f(x_k)$. 
$f_1$ and $f_2$ estimates are averaged across channels to yield the individualised alpha window that will define the frequency span for the CoG, which is calculated across all available channels (irrespective of whether they contributed to the estimation of the alpha interval).

Given that resting-state EEG is frequently recorded both before and after an experimental session, we include the capability to compute repeated-measures comparisons and pooled averages across intraindividual datasets. 
Indeed, since concerted alpha-band activity is not guaranteed to manifest during any given recording<!-- AC: ?look at pre/post papers-->, we recommend such cross-recording comparisons in order to maximise the likelihood of being able to derive reliable estimates of IAF. 
As such, we have extended the principles of our peak quality analysis so to ensure cross-recording estimates are weighted towards the recording that manifests strongest evidence of a distinct PAF (in cases where there is a disparity between recordings). 
The amount of information rendered by each recording is also accounted for insofar as recordings furnishing data from fewer channels will be assigned a lower weighting when pre- and post-session data are averaged.

### 2.3 Human EEG data

#### 2.3.1 Participants
Sixty-ish right-handed [Edinburgh Handedness Inventory; @oldfield1971], native English-speaking adults (sex, mean age, age range) with normal (or corrected-to-normal) vision and audition, and no history of psychiatric, neurological, or cognitive disorder, participated in the study. 
All participants provided written, informed consent, and received financial remuneration for their time. 
This study, which formed part of a larger research project investigating EEG responses to complex, naturalistic stimuli [@gysin-websterinprep], was approved by the University of South Australia Human Research Ethics Committee (Application ID: 0000035576).

#### 2.3.2 Procedure
Following screening and consent procedures, participants were seated in a dimly-lit, sound-attenuated room for the duration of the session. 
Two sets of resting-state EEG recordings were acquired approximately 90 min apart at the beginning and end of an experimental procedure. 
This procedure involved watching approximately 70 min of pre-recorded television programming, followed by an old/new cued recall task. 
As per our standard laboratory protocol, both sets of resting-state recordings comprised of approximately 2 min of eyes-open EEG followed by 2 min of eyes-closed EEG. 
Participants were instructed to sit still, relax, and avoid excessive eye movements during this time. 
In total, the entire session lasted between 2.5 and 3 hr. 
Note, only data from the eyes-closed component of the resting-state recordings will be reported in the analyses that follow.
Although our approach could easily be extended to the analysis of eyes-open EEG, we favour eyes-closed data on the basis that it demonstrates (1) greater interindividual variability in alpha power [@chen2008], and (2) improved within-session reliability and test-retest stability of IAF estimates [@grandy2013a; cf. @bazanova2011, as cited in @bazanova2014, p.100].
Eyes-closed recordings are also advantageous in reducing the incidence of ocular artifact.

#### 2.3.3 EEG acquisition and preprocessing
EEG was recorded continuously from 64 cap-mounted Ag/AgCl electrodes via Scan 4.5 software for the SynAmpsRT amplifier (Compumedics^&reg;^ Neuroscan&trade;, Charlotte, NC, USA). 
The online recording was digitised at a rate of 1000 Hz, bandpass filtered (passband: 0.05-200 Hz), and referenced to the vertex electrode (AFz served as the ground electrode). 
Eye movements were also recorded from bipolar channels positioned above and below the left eye, and on the outer canthi of both eyes. 
Electrode impedances were maintained below 12.5 k$\Omega$.

EEG data acquired during eyes-closed resting-state recordings were preprocessed in MATLAB version 8.3.0.532. 
First, all EEG channels were imported into the MATLAB workspace via EEGLAB version 13.6.5b and re-referenced to linked mastoids. 
Each dataset was then trimmed to retain only the nine centro-posterior electrodes that constituted the region of interest for resting-state IAF analysis: Pz, P3/4, POz, PO3/4, Oz, O1/2. 
These channels were downsampled to 250 Hz and subjected to zero-phase, finite impulse response (FIR) highpass (passband: 0.3 Hz, -6 dB cutoff: 0.15 Hz) and lowpass (passband: 30 Hz, -6 dB cutoff: 33.75 Hz), Hamming-windowed sinc filters. 
Finally, all recordings exceeding 120 s in duration were trimmed to ensure standardisation of the total quantity of data analysed across participants.
	
Note that these data were not subjected to any artifact detection/rejection procedures beyond filtering. 
We did not anticipate any undue influence from eye movements on account of (1) the posterior location of the electrode channels, (2) the likely minimisation of blinks and eye movements due to the eyes-closed nature of the recording, and (3) the low frequency (i.e. delta and theta) characteristics of ocular artifact [@gasser1992].
Muscular activity was likewise not expected to contaminate the spectral bandwidth of interest, on account of the relatively high frequency (i.e. ≥ 20 Hz) of electromyographic artifact [@muthukumaraswamy2013].
<!-- PA: might be worth cutting down on the generic methods and just referring to Jess' paper -->
#### 2.3.4 IAF analysis parameters
Initial parameters for the IAF analysis were determined on the basis of preliminary testing with an independent set of resting-state data. (These data were collected as part of a separate EEG protocol).

The length of the Hamming window implemented in `pwelch` was set at 2048 samples (4 times the sampling rate raised to the next power of 2), which yielded a frequency resolution of ~0.24 Hz. This sliding window was applied with 50% overlap. The bounds of the alpha-band window $W_\alpha$ were set at 7 and 13 Hz. Width of the SGF sliding window $Fw$ was set at 11 samples (i.e. frequency bins), corresponding to a frequency span of ~2.44 Hz. 
A fifth-degree polynomial was selected as the curve-fitting parameter $k$.

The minimum power difference parameter was set at $pDiff = .20$. This meant that the largest peak detected within $W_\alpha$ had to register a PSD value at least 20% greater than that of the next largest peak to qualify as the PAF estimate for that channel. 
Although this criterion constitutes an arbitrarily defined threshold, we know of no objective standard (or theoretical rationale) that can be used to guide the determination of this necessary boundary condition. 
However, we consider it a strength of the current approach that this limit must be explicitly defined prior to data analysis, and uniformly applied across all encountered cases.

In addition to investigating the extent to which IAF estimates (and related alpha-band parameters) varied across the sample, we also performed within-subject comparisons of pre- and post-experiment spectral data. 
For both of these intra- and interindividual analyses, the minimum number of channel-wise PSD estimates required for PAF and CoG calculation was set at $minC = 3$. 
In the event that only one of the paired recordings satisfied $minC$, IAF estimates were derived solely from this data. 
From a methodological perspective, we were interested to observe how many cases failed to satisfy criteria necessary for PAF and/or CoG estimation; and in particular, the extent to which CoG provided additional information in cases where the PAF could not be reliably ascertained.

### 2.4 Simulated EEG data
<!-- AC's notes. We present ?8 simulation datasets to assess how well the analysis routine performs across varied spectral conditions. summarise in table - 8.75, 11.25, mixed; narrow band, broad band; low noise, (medium?), high noise
random variance in the amount of noise introduced – generate 9 channels each condition and calculate estimates (I.e. create single dataset each condition)
display a random channel from each dataset – overlay genuine PSD with estimate

sample from pink noise and alpha spectrum component signals – proportion of each will define SNR. (Add random variance to both signals prior combining)


The eyes-closed resting-state EEG was recorded while the participant sat alone in a quiet, dimly lit room. All recordings were made immediately following an analogous period of eyes-open resting-state EEG (not analysed here).


## 2 Results
human eeg
grand average PSDs
correl / shared var cog vs paf [cf @jann2010]
histogram distribution of intersubject IAFs (? Variability of alpha range)
? some way of assessing variability of channel-wise ests per subject
number retained/excluded chans, sims/diffs PAF/CoG

? use biosemi data to develop programme (fine tune procedure for determining f1-f2, note need for d1 range:+/-1 with consideration of neighbours), test on larger neuroscan dataset (need reasonable number if IAF ~normally distrib). 
? ask people to assign f1/f2 to genuine data and compare (inter-rater rel)

sims
overlay estimated PSD with simulated component 
? some way of quantifying distance iaf hat from iaf? 



Plurality of alpha rhythms (klimesch papers, also sterman 96, cf basar 12, also basar 97 - diffuse alpha system; haegens 14) – cog may be more valuable for characterising iaf in way that takes distribution into account (K97?). we provide way of estimating cog that doesn’t depend on assumptions or paradigm of ERD.
baz 11 says hooper thinks PAF is best measure of interind var.

why not eyes open ? note increased variation in EC alpha vs EO suggests former may be more sensitive to ind diffs. indeed, bazanova 11 (cited in R/Vs 12/14) found evidence of best intraind correl for posterior EC IAF. 
also not evidence from curve fittin data is EC – est norms

prevalence of split peaks (44% in chiang 11, although difficult to tell if this holds for posterior chans only – note that the upper [higher freq] peak is generally more occipital/stronger, more akin to single peak cases), plus possibility single peaks could be superposed/ ‘unresolved double peaks’ (see chiang 2011:1513) – CoG esp valuable. Even the curve-fitting programmes don’t estimate (lack means of specifying f1/f2). 

note that haegens et al, who seem to be using the van albaba version of chiang’s algorithm, only get 38 out of 51 PAFs (they remark that the curve fitting method is more conservative than standard PAF approach) – can we do better (or at least, provide CoG as viable alternative)? perhaps assumption of gaussian curve is too restrictive in some cases?
[NB: remark that noisier/ambiguous peaks omitted suggests the algorithm isn’t the same, ? seem to find Gaussian peak at expense of split subpeaks]
-->