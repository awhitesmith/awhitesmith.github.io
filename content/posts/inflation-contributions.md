---
title: "Calculating contributions to inflation"
date: 2023-10-21T18:55:20+09:30
draft: false
math: true
description: "A simple method for calculating the percentage point contributions of expenditure classes to year-ended and year-average change in the consumer price index (and other chain-linked indices)."
---

This is a writeup on calculating percentage point contribution of an expenditure class to year-ended and year-average change in the Australian [consumer price index](https://www.abs.gov.au/statistics/economy/price-indexes-and-inflation/consumer-price-index-australia/latest-release) (CPI) as published by the [Australian Bureau of Statistics](https://www.abs.gov.au/) (ABS).

While this focuses on CPI specifically, the same method can be applied to any other chain linked index that can be broken down to components where it is possible to calculate the percentage point contributions of those components for the base time unit (e.g. quarterly contributions for a quarterly series). The formulas can be modified slightly to work with monthly series by replacing 3's with 11's and 4's with 12's where appropriate. With a bit of imagination you should be able to modify them to calculate contributions to change over longer or shorter time periods too.

You can find spreadsheets in this [GitHub repo](https://github.com/awhitesmith/inflation-contributions) for calculating the percentage point contributions in quarterly, year-ended and year-average terms for the capital cities and national CPI.

{{< chart 90 300 >}}
{

    type: 'bar',
    data: {
        labels: ['Jun-21', 'Sep-21', 'Dec-21', 'Mar-22', 'Jun-22', 'Sep-22', 'Dec-22', 'Mar-23', 'Jun-23', 'Sep-23', 'Dec-23'],
        datasets: [{
            label: 'Food',
            data: [0.13, 0.23, 0.32, 0.73, 0.99, 1.51, 1.54, 1.34, 1.27, 0.83, 0.76],
            backgroundColor: '#ffa62b',
            borderWidth: 0
        }, {
            label: 'Housing',
            data: [-0.04, 0.38, 0.93, 1.56, 2.08, 2.45, 2.46, 2.24, 1.83, 1.56, 1.35],
            backgroundColor: '#9daa2e',
            borderWidth: 0
        }, {
            label: 'Transport',
            data: [1.07, 1.06, 1.27, 1.4, 1.36, 0.97, 0.86, 0.47, 0.21, 0.61, 0.4],
            backgroundColor: '#449d5b',
            borderWidth: 0
        }, {
            label: 'Recreation',
            data: [0.26, 0.2, 0.19, 0.26, 0.39, 0.43, 0.92, 0.88, 0.71, 0.61, 0.06],
            backgroundColor: '#00857a',
            borderWidth: 0
        }, {
            label: 'Other',
            data: [2.43, 1.14, 0.79, 1.14, 1.32, 1.91, 2.05, 2.09, 2.01, 1.76, 1.48],
            backgroundColor: '#16697a',
            borderWidth: 0
        }]
    },
    options: {
        maintainAspectRatio: false,
        title: {
          display: true,
          text: "Contributors to year-ended inflation, Australia",
          fontSize: 16
        },
        scales: {
          xAxes: [{
            stacked: true,
            gridLines: {
              drawOnChartArea: false
            },
            ticks: {
              callback: (t, i) => i % 2 ? '' : t
            }
          }],
          yAxes: [{
            stacked: true,
            gridLines: {
              drawOnChartArea: false
            },
            ticks: {
              min: 0
            }
          }]
        },
        legend: {
          position: 'bottom'
        }
    }
}
{{< /chart >}}

## The short

Letting:

* \\(\text{CPI}\_t\\) be the CPI in time period \\(t\\)
* \\(\text{EC}^j\_t\\) be the index for the expenditure class \\(j\\) in time period \\(t\\)
* \\(W^j\_t\\) be the weight of expenditure class \\(j\\) in time period \\(t\\) (see note below on weights)
* \\(\text{RP}\_t\\) be the reference period for time period \\(t\\) (see note below on reference periods)
* \\(\text{contrib}^{\text{qtr}\vert\text{ye}\vert\text{yavg}}\_{j,t}\\) be the percentage point contribution of an expenditure class \\(j\\) to the quarterly, year-ended and year-average change in CPI respectively in time period \\(t\\)

The contribution to quarterly change in CPI in time period \\(t\\) of the expenditure class \\(j\\)

$$
\text{contrib}\_{j,t}^{\text{qtr}}=W^j\_t\cdot \frac{\text{CPI}\_{\text{RP}\_t}}{\text{CPI}\_{t-1}}\cdot \frac{\text{EC}^j\_t-\text{EC}^j\_{t-1}}{\text{EC}^j\_{\text{RP}\_t}}\tag{1}
$$

The contribution to year-ended inflation in time period \\(t\\) of the expenditure class \\(j\\) is given by

$$
\text{contrib}\_{j,t}^{\text{ye}}=\frac{1}{\text{CPI}\_{t-4}}\sum\_{i=0}^3 \text{CPI}\_{t-i-1} \cdot \text{contrib}\_{j,t-i}^{\text{qtr}}\tag{2}
$$

The contribution to year-average change in the CPI in time period \\(t\\) of the expenditure class \\(j\\) is given by

$$
\text{contrib}\_{j,t}^{\text{yavg}}=\frac{1}{\sum\limits\_{i=0}^3\text{CPI}\_{t-i-4}}\sum\_{i=0}^3 \text{CPI}\_{t-i-4} \cdot \text{contrib}\_{j,t-i}^{\text{ye}}\tag{3}
$$

### Weights and reference periods

Each expenditure class of the CPI has a weight associated with it. These weights are updated from time to time by the ABS, which necessitates chaining. The weights for expenditure classes are fixed between weight updates, which are generally four quarters apart.

The reference period for a weight update is the quarter immediately prior to the first quarter in which that weight update takes effect. For example, the weights from the 2021 weight update took effect from December 2021, so the reference period for the 2021 weight update is September 2021 (the quarter before). Like the weights themselves, the reference period is fixed between weight updates.

The table below shows an example of the weights for a few expenditure classes and the reference periods between June 2022 and December 2023.

<table>
  <thead>
    <tr>
      <th>Date</th><th>Reference period</th><th colspan="3" style="text-align: center;">Weights</th><th></th>
    </tr>
    <tr>
      <th></th><th></th><th style="font-weight:bold;">Rents</th><th style="font-weight:bold;">Utilities</th><th>Fuel</th style="font-weight:bold;"><th style="font-weight:bold;">...</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Jun-22</td><td>Sep-21</td><td>6.23</td><td>4.44</td><td>3.28</td><td>...</td>
    </tr>
    <tr>
      <td>Sep-22</td><td>Sep-21</td><td>6.23</td><td>4.44</td><td>3.28</td><td>...</td>
    </tr>
    <tr>
      <td>Dec-22</td><td>Sep-22</td><td>5.75</td><td>4.08</td><td>3.61</td><td>...</td>
    </tr>
    <tr>
      <td>Mar-23</td><td>Sep-22</td><td>5.75</td><td>4.08</td><td>3.61</td><td>...</td>
    </tr>
    <tr>
      <td>Jun-23</td><td>Sep-22</td><td>5.75</td><td>4.08</td><td>3.61</td><td>...</td>
    </tr>
    <tr>
      <td>Sep-23</td><td>Jun-23</td><td>5.76</td><td>4.22</td><td>3.46</td><td>...</td>
    </tr>
    <tr>
      <td>Dec-23</td><td>Jun-23</td><td>5.76</td><td>4.22</td><td>3.46</td><td>...</td>
    </tr>
  </tbody>
</table>


## The long

### The problem

Ordinarily, given a series that can be decomposed into a sum of components, for example \\(I\_t=A\_t+B\_t+C\_t\\), we can calculate the percentage point contributions to change of those components as below:

$$
\text{contrib}\_{A,t}^{\text{qtr}}=\frac{A\_t-A\_{t-1}}{I\_{t-1}}\tag{4}
$$

$$
\text{contrib}\_{A,t}^{\text{ye}}=\frac{A\_t-A\_{t-4}}{I\_{t-4}}\tag{5}
$$

$$
\text{contrib}\_{A,t}^{\text{yavg}}=\frac{\sum\limits\_{i=0}^3 (A\_{t-i}-A\_{t-i-4})}{\sum\limits\_{i=0}^3I\_{t-i-4}}\tag{6}
$$

CPI can be decomposed into a sum of its components as follows:

$$
\text{CPI}\_t=\sum\limits\_{j} W\_t^j\cdot \text{CPI}\_{\text{RP}\_t}\cdot \frac{\text{EC}^j\_t}{\text{EC}^j\_{\text{RP}\_t}}\tag{7}
$$

Trying to apply the year-ended contribution formula in (5) for an expenditure class \\(j\\) gives the following result:

$$
\text{contrib}\_{j,t}^{\text{ye}}=\frac{1}{\text{CPI}\_{t-4}}\cdot \left(W\_t^j\cdot \text{CPI}\_{\text{RP}\_t}\cdot \frac{\text{EC}^j\_t}{\text{EC}^j\_{\text{RP}\_t}}- W\_{t-4}^j\cdot \text{CPI}\_{\text{RP}\_{t-4}}\cdot \frac{\text{EC}^j\_{t-4}}{\text{EC}^j\_{\text{RP}\_{t-4}}}\right)\tag{8}
$$

However this formula has a few undesirable properties. For example, consider the below table containing the year-ended percentage point contributions of the rents expenditure class to Australian CPI calculated using this method. The table shows that rents contributed negatively to year-ended change in CPI from December 2021 to September 2022, despite there not being a single quarter of negative rents growth from March 2021 onwards. There are also sudden changes in the year-ended contributions during weight updates in December 2021 and December 2022 that do not appear to be related to changes in the rents index.

<table>
<thead>
  <tr>
    <th  colspan="4" style="text-align: center;">Rents</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td ></td><td style="font-weight:bold;">Index</td><td style="font-weight:bold;">Weight</td><td style="font-weight:bold;">Year-ended contribution</td>
  </tr>
  <tr>
    <td >Dec-20</td><td >110.9</td><td >6.80</td><td >0.01</td>
  </tr>
  <tr>
    <td >Mar-21</td><td >110.9</td><td >6.80</td><td >0.01</td>
  </tr>
  <tr>
    <td >Jun-21</td><td >111.0</td><td >6.80</td><td >0.11</td>
  </tr>
  <tr>
    <td >Sep-21</td><td >111.2</td><td >6.80</td><td >0.13</td>
  </tr>
  <tr>
    <td >Dec-21</td><td >111.3</td><td >6.23</td><td >-0.38</td>
  </tr>
  <tr>
    <td >Mar-22</td><td >112.0</td><td >6.23</td><td >-0.34</td>
  </tr>
  <tr>
    <td >Jun-22</td><td >112.8</td><td >6.23</td><td >-0.30</td>
  </tr>
  <tr>
    <td >Sep-22</td><td >114.3</td><td >6.23</td><td >-0.22</td>
  </tr>
  <tr>
    <td >Dec-22</td><td >115.7</td><td >5.75</td><td >0.01</td>
  </tr>
</tbody>
</table>

### A solution

The CPI is a chain linked series. This means it is actually several series that have been stitched together ("chain linked"). The underlying series overlap by one period to accommodate this, like in the below table:

<table>
<thead>
  <tr>
    <th ></th><th >t</th><th >t+1</th><th >t+2</th><th >t+3</th><th >t+4</th><th >t+5</th><th >t+6</th><th >t+7</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td >Series 1</td><td >100.0</td><td >101.2</td><td >102.9</td><td ></td><td ></td><td ></td><td ></td><td ></td>
  </tr>
  <tr>
    <td >Series 2</td><td ></td><td ></td><td >100.0</td><td >100.2</td><td >101.1</td><td >101.4</td><td >101.9</td><td ></td>
  </tr>
  <tr>
    <td >Series 3</td><td ></td><td ></td><td ></td><td ></td><td ></td><td ></td><td >100.0</td><td >100.8</td>
  </tr>
</tbody>
</table>

The reference periods discussed separately above correspond to the start of each new series (i.e. the overlapping period). Based on that fact, the following holds:

$$
\text{CPI}\_{t-1}=\sum\limits\_{j} W\_t^j\cdot \text{CPI}\_{\text{RP}\_t}\cdot \frac{\text{EC}^j\_{t-1}}{\text{EC}^j\_{\text{RP}\_t}}\tag{9}
$$

This is clear when \\(t-1\\) is not a reference period, because in that case \\(\text{RP}\_t=\text{RP}\_{t-1}\\) and \\(\text{W}\_t=\text{W}\_{t-1}\\) which means (9) is just a restatement of (7). If \\(t-1\\) is a reference period we can note that \\(\text{RP}\_t=t-1\\), and therefore the sum in (9) simplifies to \\(\sum\_j\text{W}^j\_t\cdot\text{CPI}\_{t-1}\\). The weights sum to 1 and we are left with \\(\text{CPI}\_{t-1}\\).

Given (9) we can write \\(\Delta\text{CPI}\_t=\text{CPI}\_t-\text{CPI}\_{t-1}\\) as follows:

$$
\Delta\text{CPI}\_t=\sum\limits\_{j} W\_t^j\cdot \text{CPI}\_{\text{RP}\_t}\cdot \frac{\text{EC}^j\_t-\text{EC}^j\_{t-1}}{\text{EC}^j\_{\text{RP}\_t}}\tag{10}
$$

Using \\(\text{CPI}\_t=\text{CPI}\_0+\sum\_{i=1}^t\Delta\text{CPI}\_i\\), we can substitute in (10) and swap around the sums to get the following expression for \\(\text{CPI}\_t\\):

$$
\text{CPI}\_t=\text{CPI}\_0+\sum\limits\_{j}\left(\sum\_{i=1}^t W\_t^j\cdot \text{CPI}\_{\text{RP}\_t}\cdot \frac{\text{EC}^j\_t-\text{EC}^j\_{t-1}}{\text{EC}^j\_{\text{RP}\_t}}\right)\tag{11}
$$

And we can then note that we've decomposed CPI into a sum of time series, which can be plugged into (4), (5) and (6) to give (1), (2) and (3) respectively.

Using these equations to recalculate the year-ended percentage point contributions for rents, we can see much nicer behaviour:

<table >
<thead>
  <tr>
    <th  colspan="4" style="text-align: center; font-weight: bold;"><strong>Rents</strong></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td ></td><td style="font-weight:bold;">Index</td><td style="font-weight:bold;">Weight</td><td style="font-weight:bold;">Year-ended contribution</td>
  </tr>
  <tr>
    <td >Dec-20</td><td >110.9</td><td >6.80</td><td >-0.09</td>
  </tr>
  <tr>
    <td >Mar-21</td><td >110.9</td><td >6.80</td><td >-0.10</td>
  </tr>
  <tr>
    <td >Jun-21</td><td >111.0</td><td >6.80</td><td >0.00</td>
  </tr>
  <tr>
    <td >Sep-21</td><td >111.2</td><td >6.80</td><td >0.02</td>
  </tr>
  <tr>
    <td >Dec-21</td><td >111.3</td><td >6.23</td><td >0.02</td>
  </tr>
  <tr>
    <td >Mar-22</td><td >112.0</td><td >6.23</td><td >0.06</td>
  </tr>
  <tr>
    <td >Jun-22</td><td >112.8</td><td >6.23</td><td >0.10</td>
  </tr>
  <tr>
    <td >Sep-22</td><td >114.3</td><td >6.23</td><td >0.17</td>
  </tr>
  <tr>
    <td >Dec-22</td><td >115.7</td><td >5.75</td><td >0.24</td>
  </tr>
</tbody>
</table>

## Other reading

* See [this paper](https://www.oecd.org/sdd/prices-ppp/OECD-calculation-contributions-annual-inflation.pdf) from the OECD for an equivalent method of calculating year-ended contributions for series with annually updated weights. Might need some extra work to apply this to the Australian CPI as the paper only covers the case of a single weight update in a year (noting the year to March 2024 has two weight changes due to an ad-hoc [partial update](https://www.abs.gov.au/statistics/economy/price-indexes-and-inflation/consumer-price-index-australia/sep-quarter-2023#what-s-new-this-quarter) for the September quarter 2023).
