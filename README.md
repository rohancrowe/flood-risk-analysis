# flood-risk-analysis
How can we calculate probabilities of different flood events? This project uses a peaks over threshold approach to clean and fit probability distributions to time series from UK river height data.

## objectives

## methodology

[Pickands-Balkema-de Haan theorem](https://en.wikipedia.org/wiki/Pickands%E2%80%93Balkema%E2%80%93De_Haan_theorem) is like a CLT for threshold probabilities, ie probabilities of the form:

F(y) = P(X-u ≤ y | X > u) 

u is the threshold (in peaks over threshold), X is a random variable whose distribution we don't know

It states the distribution above is well approximated by a [generalised Pareto distribution](https://en.wikipedia.org/wiki/Generalized_Pareto_distribution)

### data processing

![Overall River Flow Time Series](flood-risk-analysis/results/figures/overall_time_series.png)

As we can see, data from all of 1977 and 1978 is missing (is confirmed in the code). We need to look at the data from the end of 1976 and the start of 1979. If it looks like we are in the vicinity of a peak there, it may be necessary to also remove this peak from the dataset, as we do not know what the behaviour of the peak was like over when we move over the cut line. There is not much hope in analytically continuing the time series due to the high level of uncertainty in metereological effects that we would have to consider.

![1977 Cut Analysis](flood-risk-analysis/results/figures/1977_cut_analysis.png)
![1978 Cut Analysis](flood-risk-analysis/results/figures/1978_cut_analysis.png)

![2015 Analysis](flood-risk-analysis/results/figures/2015_analysis.png)

![1959 Edge Analysis](flood-risk-analysis/results/figures/1959_edge_analysis.png)

Independence criteria:

We will follow the UK Centre for Ecology & Hydrology standard test for independence. See page 276 of the Flood Estimation Handbook Volume 3 Statistical procedures for flood frequency estimation [linked at the bottom of this page](https://www.ceh.ac.uk/data/software-models/flood-estimation-handbook).

Test 1: "The two peaks must be separated by at least three times the average time to rise." 
Test 2: "The minimum discharge in the trough between two peaks must be less than two-thirds of the discharge of the first of the two peaks." 

The average time to rise is calculated by taking the mean of the time from trough to peak for all candidate single (ie not multi-) peaks and is reflective of how quick an independent rain event turns into an independent flood event based on the nearby water table, geology etc... This suggests the following algorithm:

1. Find all peaks
2. Choose a threshold that has on average 5 peaks over the threshold per year
3. Subject these peaks to independence tests, if a peak fails an independence test, remove it from the dataset and go back to step 2

Test 1 is hard to implement. How do we calculate any single "time to rise"? Using guidance from the Flood Estimation Handbook, we find n single-peaked events, find their time to rise, and average them. To calculate a single-peaked time to rise we look at the closest previous trough and the peak and find the time between them. We'll define a single peaked event as having its two neighbouring troughs below the mean flow rate of the river, ie back to normal river flow behaviour. The figure below should justify this working:

![2010 River Flow](flood-risk-analysis/results/2010.png)

We can see single-peaked events have neighbouring troughs that are below the mean flow rate.



### distribution fitting

### statistical tests

![QQ Plot](flood-risk-analysis/results/QQ_plot.png)

## results

## data

The project analyses river flow data from the UK National River Flow Archive (NRFA).

You can download the data from:
Lune at Caton: https://nrfa.ceh.ac.uk/data/station/meanflow/72004

Data are provided by the UK Centre for Ecology & Hydrology (UKCEH) and are subject to the Open Government Licence v3.0. See:
https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/

Raw data should be downloaded directly from NRFA.

## installation

Clone the repository:

```bash
git clone git@github.com:yourusername/flood-frequency-analysis.git
```