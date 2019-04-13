

```python
import pandas as pd
from sqlalchemy import create_engine
```


```python
# Read depression csv and create df
csv_file = "./Resources/adult-depression-lghc-indicator-1.csv"
depression_df = pd.read_csv(csv_file, encoding="latin1")
depression_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Strata</th>
      <th>Strata Name</th>
      <th>Frequency</th>
      <th>Weighted Frequency</th>
      <th>Percent</th>
      <th>Lower 95% CL</th>
      <th>Upper 95% CL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2012</td>
      <td>Total</td>
      <td>Total</td>
      <td>1920</td>
      <td>NaN</td>
      <td>11.74</td>
      <td>11.11</td>
      <td>12.37</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2012</td>
      <td>Sex</td>
      <td>Male</td>
      <td>561</td>
      <td>1116664.0</td>
      <td>8.12</td>
      <td>7.32</td>
      <td>8.92</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2012</td>
      <td>Sex</td>
      <td>Female</td>
      <td>1359</td>
      <td>2163108.0</td>
      <td>15.25</td>
      <td>14.30</td>
      <td>16.20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012</td>
      <td>Race-Ethnicity</td>
      <td>White</td>
      <td>1314</td>
      <td>1806371.0</td>
      <td>14.57</td>
      <td>13.67</td>
      <td>15.46</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2012</td>
      <td>Race-Ethnicity</td>
      <td>Black</td>
      <td>97</td>
      <td>222022.0</td>
      <td>13.54</td>
      <td>10.44</td>
      <td>16.65</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Read obesity csv and create df
csv_file_2 = "./Resources/Obesity_in_California__2012_and_2013.csv"
obesity_df = pd.read_csv(csv_file_2, encoding="latin1")
obesity_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Age Group</th>
      <th>Category</th>
      <th>Type</th>
      <th>Obese (%)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2012</td>
      <td>Adolescent (12-17)</td>
      <td>All</td>
      <td>Total</td>
      <td>11.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2012</td>
      <td>Adolescent (12-17)</td>
      <td>Gender</td>
      <td>Male</td>
      <td>14.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2012</td>
      <td>Adolescent (12-17)</td>
      <td>Gender</td>
      <td>Female</td>
      <td>7.8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012</td>
      <td>Adolescent (12-17)</td>
      <td>Gender by Age- Male</td>
      <td>12-13</td>
      <td>15.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2012</td>
      <td>Adolescent (12-17)</td>
      <td>Gender by Age- Male</td>
      <td>14-15</td>
      <td>14.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Rename depression df columns
depression = depression_df.rename(columns = {"Year": "study_year",
                                            "Strata":"strata", "Strata Name":"strata_type",
                                            "Frequency":"frequency","Weighted Frequency":"weighted_frequency",
                                           "Percent":"percent", "Lower 95% CL":"lower_95", "Upper 95% CL":"upper_95"})
depression.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2012</td>
      <td>Total</td>
      <td>Total</td>
      <td>1920</td>
      <td>NaN</td>
      <td>11.74</td>
      <td>11.11</td>
      <td>12.37</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2012</td>
      <td>Sex</td>
      <td>Male</td>
      <td>561</td>
      <td>1116664.0</td>
      <td>8.12</td>
      <td>7.32</td>
      <td>8.92</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2012</td>
      <td>Sex</td>
      <td>Female</td>
      <td>1359</td>
      <td>2163108.0</td>
      <td>15.25</td>
      <td>14.30</td>
      <td>16.20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012</td>
      <td>Race-Ethnicity</td>
      <td>White</td>
      <td>1314</td>
      <td>1806371.0</td>
      <td>14.57</td>
      <td>13.67</td>
      <td>15.46</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2012</td>
      <td>Race-Ethnicity</td>
      <td>Black</td>
      <td>97</td>
      <td>222022.0</td>
      <td>13.54</td>
      <td>10.44</td>
      <td>16.65</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Rename obesity df columns
obesity = obesity_df.rename(columns = {"Year": "study_year", "Age Group":"age_group",
                                            "Category":"strata", "Type":"strata_type",
                                            "Obese (%)":"obesity"})
obesity.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>age_group</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>obesity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2012</td>
      <td>Adolescent (12-17)</td>
      <td>All</td>
      <td>Total</td>
      <td>11.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2012</td>
      <td>Adolescent (12-17)</td>
      <td>Gender</td>
      <td>Male</td>
      <td>14.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2012</td>
      <td>Adolescent (12-17)</td>
      <td>Gender</td>
      <td>Female</td>
      <td>7.8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012</td>
      <td>Adolescent (12-17)</td>
      <td>Gender by Age- Male</td>
      <td>12-13</td>
      <td>15.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2012</td>
      <td>Adolescent (12-17)</td>
      <td>Gender by Age- Male</td>
      <td>14-15</td>
      <td>14.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Filter by study year (2012-2013) for obesity df
obesity = obesity.loc[obesity['study_year'].isin([2013])]
obesity
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>age_group</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>obesity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>24</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>All</td>
      <td>Total</td>
      <td>30.2</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Gender</td>
      <td>Male</td>
      <td>26.5</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Gender</td>
      <td>Female</td>
      <td>33.9</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>18 - 24</td>
      <td>21.1</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>25 - 34</td>
      <td>30.6</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>35 - 50</td>
      <td>33.4</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>51 - 64</td>
      <td>35.2</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>65+</td>
      <td>25.2</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>White</td>
      <td>27.1</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>African American</td>
      <td>44.2</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>Latino</td>
      <td>34.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>Asian/Other</td>
      <td>23.9</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>Less than High School</td>
      <td>31.9</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>High School Graduate</td>
      <td>32.7</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>Some College</td>
      <td>33.8</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>College Graduate</td>
      <td>23.1</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>Less than $15,000</td>
      <td>32.4</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$15,000 - 24,999</td>
      <td>37.7</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$25,000 - 34,999</td>
      <td>28.1</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$35,000 - 49,999</td>
      <td>28.2</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$50,000+</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>45</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Physical Activity</td>
      <td>Met Aerobic Recommendation</td>
      <td>27.5</td>
    </tr>
    <tr>
      <th>46</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Physical Activity</td>
      <td>Did Not Meet Aerobic Recommendation</td>
      <td>34.6</td>
    </tr>
    <tr>
      <th>47</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Participant</td>
      <td>34.4</td>
    </tr>
    <tr>
      <th>48</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Likely Eligible, â¤ 130%</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>49</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Not Eligible, &gt; 185%</td>
      <td>20.9</td>
    </tr>
    <tr>
      <th>50</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>All</td>
      <td>Total</td>
      <td>27.1</td>
    </tr>
    <tr>
      <th>51</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Gender</td>
      <td>Male</td>
      <td>32.6</td>
    </tr>
    <tr>
      <th>52</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Gender</td>
      <td>Female</td>
      <td>21.5</td>
    </tr>
    <tr>
      <th>53</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Age</td>
      <td>6-8</td>
      <td>32.6</td>
    </tr>
    <tr>
      <th>54</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Age</td>
      <td>9-11</td>
      <td>21.5</td>
    </tr>
    <tr>
      <th>55</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Ethnicity</td>
      <td>White</td>
      <td>23.2</td>
    </tr>
    <tr>
      <th>56</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Ethnicity</td>
      <td>African American</td>
      <td>30.4</td>
    </tr>
    <tr>
      <th>57</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Ethnicity</td>
      <td>Latino</td>
      <td>27.9</td>
    </tr>
    <tr>
      <th>58</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Ethnicity</td>
      <td>Asian/Other</td>
      <td>19.4</td>
    </tr>
    <tr>
      <th>59</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Parent Education</td>
      <td>Less than High School</td>
      <td>26.8</td>
    </tr>
    <tr>
      <th>60</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Parent Education</td>
      <td>High School Graduate</td>
      <td>32.1</td>
    </tr>
    <tr>
      <th>61</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Parent Education</td>
      <td>Some College/Graduate</td>
      <td>25.3</td>
    </tr>
    <tr>
      <th>62</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Fruits and Vegetables</td>
      <td>Met MyPlate (2Â½-5 cups)</td>
      <td>25.3</td>
    </tr>
    <tr>
      <th>63</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Fruits and Vegetables</td>
      <td>Below Guidelines</td>
      <td>27.4</td>
    </tr>
    <tr>
      <th>64</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Physical Activity</td>
      <td>&gt; 60 minutes</td>
      <td>30.7</td>
    </tr>
    <tr>
      <th>65</th>
      <td>2013</td>
      <td>Child (6-11)</td>
      <td>Physical Activity</td>
      <td>&lt; 60 minutes</td>
      <td>22.4</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Filter by study year (2012-2013) for depression df
depression = depression.loc[depression['study_year'].isin([2013])]
depression
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23</th>
      <td>2013</td>
      <td>Total</td>
      <td>Total</td>
      <td>1689</td>
      <td>NaN</td>
      <td>13.08</td>
      <td>12.33</td>
      <td>13.82</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2013</td>
      <td>Sex</td>
      <td>Male</td>
      <td>539</td>
      <td>1307668.0</td>
      <td>9.53</td>
      <td>8.53</td>
      <td>10.52</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2013</td>
      <td>Sex</td>
      <td>Female</td>
      <td>1150</td>
      <td>2337817.0</td>
      <td>16.52</td>
      <td>15.42</td>
      <td>17.62</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>White</td>
      <td>1103</td>
      <td>1979888.0</td>
      <td>15.97</td>
      <td>14.91</td>
      <td>17.02</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Black</td>
      <td>93</td>
      <td>252871.0</td>
      <td>15.46</td>
      <td>11.91</td>
      <td>19.02</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Hispanic</td>
      <td>403</td>
      <td>1011594.0</td>
      <td>10.96</td>
      <td>9.72</td>
      <td>12.20</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Asian/Pacific Islander</td>
      <td>62</td>
      <td>308822.0</td>
      <td>7.38</td>
      <td>5.26</td>
      <td>9.50</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Other</td>
      <td>28</td>
      <td>92309.0</td>
      <td>21.63</td>
      <td>12.57</td>
      <td>30.69</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2013</td>
      <td>Education</td>
      <td>No High School Diploma</td>
      <td>222</td>
      <td>510371.0</td>
      <td>12.84</td>
      <td>10.87</td>
      <td>14.81</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2013</td>
      <td>Education</td>
      <td>High School Graduate or GED Certificate</td>
      <td>361</td>
      <td>841995.0</td>
      <td>14.28</td>
      <td>12.50</td>
      <td>16.06</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2013</td>
      <td>Education</td>
      <td>Some College or Tech School</td>
      <td>504</td>
      <td>1069791.0</td>
      <td>15.37</td>
      <td>13.81</td>
      <td>16.93</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2013</td>
      <td>Education</td>
      <td>College Graduate or Post Grad</td>
      <td>583</td>
      <td>1170903.0</td>
      <td>11.45</td>
      <td>10.31</td>
      <td>12.59</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2013</td>
      <td>Income</td>
      <td>&lt; $20,000</td>
      <td>600</td>
      <td>1286100.0</td>
      <td>18.63</td>
      <td>16.89</td>
      <td>20.38</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2013</td>
      <td>Income</td>
      <td>$20,000 - $34,999</td>
      <td>283</td>
      <td>560739.0</td>
      <td>14.57</td>
      <td>12.50</td>
      <td>16.64</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2013</td>
      <td>Income</td>
      <td>$35,000 - $49,999</td>
      <td>149</td>
      <td>305677.0</td>
      <td>11.29</td>
      <td>8.89</td>
      <td>13.68</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013</td>
      <td>Income</td>
      <td>$50,000 - $74,999</td>
      <td>186</td>
      <td>394993.0</td>
      <td>12.26</td>
      <td>10.17</td>
      <td>14.34</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2013</td>
      <td>Income</td>
      <td>$75,000 - $99,999</td>
      <td>155</td>
      <td>321528.0</td>
      <td>12.04</td>
      <td>9.88</td>
      <td>14.20</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Income</td>
      <td>$100,000+</td>
      <td>213</td>
      <td>541245.0</td>
      <td>9.89</td>
      <td>8.34</td>
      <td>11.44</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2013</td>
      <td>Age</td>
      <td>18 to 34</td>
      <td>241</td>
      <td>916614.0</td>
      <td>9.96</td>
      <td>8.53</td>
      <td>11.39</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Age</td>
      <td>35 to 44</td>
      <td>198</td>
      <td>538114.0</td>
      <td>10.42</td>
      <td>8.84</td>
      <td>11.99</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2013</td>
      <td>Age</td>
      <td>45 to 54</td>
      <td>347</td>
      <td>880554.0</td>
      <td>16.81</td>
      <td>14.85</td>
      <td>18.77</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2013</td>
      <td>Age</td>
      <td>55 to 64</td>
      <td>456</td>
      <td>741063.0</td>
      <td>18.39</td>
      <td>16.60</td>
      <td>20.18</td>
    </tr>
    <tr>
      <th>45</th>
      <td>2013</td>
      <td>Age</td>
      <td>65+ years</td>
      <td>447</td>
      <td>569139.0</td>
      <td>13.41</td>
      <td>12.08</td>
      <td>14.75</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Filter by age group (Adult 18+) for obesity df
new_obesity = obesity.loc[obesity['age_group'] == "Adult (18+)"]
new_obesity
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>age_group</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>obesity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>24</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>All</td>
      <td>Total</td>
      <td>30.2</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Gender</td>
      <td>Male</td>
      <td>26.5</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Gender</td>
      <td>Female</td>
      <td>33.9</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>18 - 24</td>
      <td>21.1</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>25 - 34</td>
      <td>30.6</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>35 - 50</td>
      <td>33.4</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>51 - 64</td>
      <td>35.2</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>65+</td>
      <td>25.2</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>White</td>
      <td>27.1</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>African American</td>
      <td>44.2</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>Latino</td>
      <td>34.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>Asian/Other</td>
      <td>23.9</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>Less than High School</td>
      <td>31.9</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>High School Graduate</td>
      <td>32.7</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>Some College</td>
      <td>33.8</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>College Graduate</td>
      <td>23.1</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>Less than $15,000</td>
      <td>32.4</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$15,000 - 24,999</td>
      <td>37.7</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$25,000 - 34,999</td>
      <td>28.1</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$35,000 - 49,999</td>
      <td>28.2</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$50,000+</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>45</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Physical Activity</td>
      <td>Met Aerobic Recommendation</td>
      <td>27.5</td>
    </tr>
    <tr>
      <th>46</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Physical Activity</td>
      <td>Did Not Meet Aerobic Recommendation</td>
      <td>34.6</td>
    </tr>
    <tr>
      <th>47</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Participant</td>
      <td>34.4</td>
    </tr>
    <tr>
      <th>48</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Likely Eligible, â¤ 130%</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>49</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Not Eligible, &gt; 185%</td>
      <td>20.9</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Get age group (Adult 18+) rows in obesity df to change
obesity_change1 = obesity.loc[obesity['strata_type'].isin(["18 - 24", "25 - 34", "35 - 50", "51 - 64"])]
obesity_change1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>age_group</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>obesity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>27</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>18 - 24</td>
      <td>21.1</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>25 - 34</td>
      <td>30.6</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>35 - 50</td>
      <td>33.4</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>51 - 64</td>
      <td>35.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create new rows that combine data (average)
obesity_change1.loc[31] = [2013, "Adult (18+)", "Age", "18 - 34", (obesity_change1.loc[27,'obesity'] + obesity_change1.loc[28,'obesity'])/2]
obesity_change1.loc[32] = [2013, "Adult (18+)", "Age", "35 - 64", (obesity_change1.loc[29,'obesity'] + obesity_change1.loc[30,'obesity'])/2]
obesity_change1
```

    C:\Users\Xero\anaconda\Anaconda3\envs\PythonData\lib\site-packages\ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    C:\Users\Xero\anaconda\Anaconda3\envs\PythonData\lib\site-packages\ipykernel_launcher.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      This is separate from the ipykernel package so we can avoid doing imports until
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>age_group</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>obesity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>27</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>18 - 24</td>
      <td>21.10</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>25 - 34</td>
      <td>30.60</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>35 - 50</td>
      <td>33.40</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>51 - 64</td>
      <td>35.20</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>18 - 34</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>35 - 64</td>
      <td>34.30</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Remove old age group rows (27, 28, 29, 30), and insert new rows into table (add 27, 28). Sort index. 
new_obesity2 = new_obesity.drop([27, 28, 29, 30])
new_obesity2.loc[27] = obesity_change1.loc[31]
new_obesity2.loc[28] = obesity_change1.loc[32]
new_obesity2 = new_obesity2.sort_index()
new_obesity2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>age_group</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>obesity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>24</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>All</td>
      <td>Total</td>
      <td>30.20</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Gender</td>
      <td>Male</td>
      <td>26.50</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Gender</td>
      <td>Female</td>
      <td>33.90</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>18 - 34</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>35 - 64</td>
      <td>34.30</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>65+</td>
      <td>25.20</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>White</td>
      <td>27.10</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>African American</td>
      <td>44.20</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>Latino</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>Asian/Other</td>
      <td>23.90</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>Less than High School</td>
      <td>31.90</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>High School Graduate</td>
      <td>32.70</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>Some College</td>
      <td>33.80</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>College Graduate</td>
      <td>23.10</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>Less than $15,000</td>
      <td>32.40</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$15,000 - 24,999</td>
      <td>37.70</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$25,000 - 34,999</td>
      <td>28.10</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$35,000 - 49,999</td>
      <td>28.20</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$50,000+</td>
      <td>24.00</td>
    </tr>
    <tr>
      <th>45</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Physical Activity</td>
      <td>Met Aerobic Recommendation</td>
      <td>27.50</td>
    </tr>
    <tr>
      <th>46</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Physical Activity</td>
      <td>Did Not Meet Aerobic Recommendation</td>
      <td>34.60</td>
    </tr>
    <tr>
      <th>47</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Participant</td>
      <td>34.40</td>
    </tr>
    <tr>
      <th>48</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Likely Eligible, â¤ 130%</td>
      <td>30.00</td>
    </tr>
    <tr>
      <th>49</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Not Eligible, &gt; 185%</td>
      <td>20.90</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Get income rows in obesity df to change
obesity_change2 = obesity.loc[obesity['strata_type'].isin(["Less than $15,000", "$15,000 - 24,999", "$25,000 - 34,999"])]
obesity_change2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>age_group</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>obesity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>Less than $15,000</td>
      <td>32.4</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$15,000 - 24,999</td>
      <td>37.7</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$25,000 - 34,999</td>
      <td>28.1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create new row that combines data (average)
obesity_change2.loc[43] = [2013, "Adult (18+)", "Income", "< $35,000", (obesity_change2.loc[40,'obesity'] + obesity_change2.loc[41,'obesity'] + obesity_change2.loc[42,'obesity'])/3]
obesity_change2
```

    C:\Users\Xero\anaconda\Anaconda3\envs\PythonData\lib\site-packages\ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>age_group</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>obesity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>Less than $15,000</td>
      <td>32.400000</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$15,000 - 24,999</td>
      <td>37.700000</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$25,000 - 34,999</td>
      <td>28.100000</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>&lt; $35,000</td>
      <td>32.733333</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Remove old rows (40, 41, 42), and insert new row into table (add 40). Sort index. 
new_obesity3 = new_obesity2.drop([40, 41, 42])
new_obesity3.loc[40] = obesity_change2.loc[43]
edited_obesity = new_obesity3.sort_index()
edited_obesity
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>age_group</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>obesity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>24</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>All</td>
      <td>Total</td>
      <td>30.200000</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Gender</td>
      <td>Male</td>
      <td>26.500000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Gender</td>
      <td>Female</td>
      <td>33.900000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>18 - 34</td>
      <td>25.850000</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>35 - 64</td>
      <td>34.300000</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>65+</td>
      <td>25.200000</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>White</td>
      <td>27.100000</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>African American</td>
      <td>44.200000</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>Latino</td>
      <td>34.000000</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>Asian/Other</td>
      <td>23.900000</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>Less than High School</td>
      <td>31.900000</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>High School Graduate</td>
      <td>32.700000</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>Some College</td>
      <td>33.800000</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>College Graduate</td>
      <td>23.100000</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>&lt; $35,000</td>
      <td>32.733333</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$35,000 - 49,999</td>
      <td>28.200000</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$50,000+</td>
      <td>24.000000</td>
    </tr>
    <tr>
      <th>45</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Physical Activity</td>
      <td>Met Aerobic Recommendation</td>
      <td>27.500000</td>
    </tr>
    <tr>
      <th>46</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Physical Activity</td>
      <td>Did Not Meet Aerobic Recommendation</td>
      <td>34.600000</td>
    </tr>
    <tr>
      <th>47</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Participant</td>
      <td>34.400000</td>
    </tr>
    <tr>
      <th>48</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Likely Eligible, â¤ 130%</td>
      <td>30.000000</td>
    </tr>
    <tr>
      <th>49</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Not Eligible, &gt; 185%</td>
      <td>20.900000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Get race/ethnicity rows in depression df to change
depression_change1 = depression.loc[depression['strata_type'].isin(["Asian/Pacific Islander", "Other"])]
depression_change1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>29</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Asian/Pacific Islander</td>
      <td>62</td>
      <td>308822.0</td>
      <td>7.38</td>
      <td>5.26</td>
      <td>9.50</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Other</td>
      <td>28</td>
      <td>92309.0</td>
      <td>21.63</td>
      <td>12.57</td>
      <td>30.69</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create new row that combines data (frequncy sum and average for other stats)
depression_change1.loc[31] = [2013, "Race-Ethnicity", "Asian/Other",
    depression_change1.loc[29,'frequency'] + depression_change1.loc[30,'frequency'],
    (depression_change1.loc[29,'weighted_frequency'] + depression_change1.loc[30,'weighted_frequency'])/2,
    (depression_change1.loc[29,'percent'] + depression_change1.loc[30,'percent'])/2,
    (depression_change1.loc[29,'lower_95'] + depression_change1.loc[30,'lower_95'])/2,
    (depression_change1.loc[29,'upper_95'] + depression_change1.loc[30,'upper_95'])/2,
    ]
depression_change1
```

    C:\Users\Xero\anaconda\Anaconda3\envs\PythonData\lib\site-packages\ipykernel_launcher.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      import sys
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>29</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Asian/Pacific Islander</td>
      <td>62</td>
      <td>308822.0</td>
      <td>7.380</td>
      <td>5.260</td>
      <td>9.500</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Other</td>
      <td>28</td>
      <td>92309.0</td>
      <td>21.630</td>
      <td>12.570</td>
      <td>30.690</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Asian/Other</td>
      <td>90</td>
      <td>200565.5</td>
      <td>14.505</td>
      <td>8.915</td>
      <td>20.095</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Remove old rows (29, 30), and insert new row into table (add 29). Sort index. 
new_depression = depression.drop([29, 30])
new_depression.loc[29] = depression_change1.loc[31]
new_depression = new_depression.sort_index()
new_depression
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23</th>
      <td>2013</td>
      <td>Total</td>
      <td>Total</td>
      <td>1689</td>
      <td>NaN</td>
      <td>13.080</td>
      <td>12.330</td>
      <td>13.820</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2013</td>
      <td>Sex</td>
      <td>Male</td>
      <td>539</td>
      <td>1307668.0</td>
      <td>9.530</td>
      <td>8.530</td>
      <td>10.520</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2013</td>
      <td>Sex</td>
      <td>Female</td>
      <td>1150</td>
      <td>2337817.0</td>
      <td>16.520</td>
      <td>15.420</td>
      <td>17.620</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>White</td>
      <td>1103</td>
      <td>1979888.0</td>
      <td>15.970</td>
      <td>14.910</td>
      <td>17.020</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Black</td>
      <td>93</td>
      <td>252871.0</td>
      <td>15.460</td>
      <td>11.910</td>
      <td>19.020</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Hispanic</td>
      <td>403</td>
      <td>1011594.0</td>
      <td>10.960</td>
      <td>9.720</td>
      <td>12.200</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Asian/Other</td>
      <td>90</td>
      <td>200565.5</td>
      <td>14.505</td>
      <td>8.915</td>
      <td>20.095</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2013</td>
      <td>Education</td>
      <td>No High School Diploma</td>
      <td>222</td>
      <td>510371.0</td>
      <td>12.840</td>
      <td>10.870</td>
      <td>14.810</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2013</td>
      <td>Education</td>
      <td>High School Graduate or GED Certificate</td>
      <td>361</td>
      <td>841995.0</td>
      <td>14.280</td>
      <td>12.500</td>
      <td>16.060</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2013</td>
      <td>Education</td>
      <td>Some College or Tech School</td>
      <td>504</td>
      <td>1069791.0</td>
      <td>15.370</td>
      <td>13.810</td>
      <td>16.930</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2013</td>
      <td>Education</td>
      <td>College Graduate or Post Grad</td>
      <td>583</td>
      <td>1170903.0</td>
      <td>11.450</td>
      <td>10.310</td>
      <td>12.590</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2013</td>
      <td>Income</td>
      <td>&lt; $20,000</td>
      <td>600</td>
      <td>1286100.0</td>
      <td>18.630</td>
      <td>16.890</td>
      <td>20.380</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2013</td>
      <td>Income</td>
      <td>$20,000 - $34,999</td>
      <td>283</td>
      <td>560739.0</td>
      <td>14.570</td>
      <td>12.500</td>
      <td>16.640</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2013</td>
      <td>Income</td>
      <td>$35,000 - $49,999</td>
      <td>149</td>
      <td>305677.0</td>
      <td>11.290</td>
      <td>8.890</td>
      <td>13.680</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013</td>
      <td>Income</td>
      <td>$50,000 - $74,999</td>
      <td>186</td>
      <td>394993.0</td>
      <td>12.260</td>
      <td>10.170</td>
      <td>14.340</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2013</td>
      <td>Income</td>
      <td>$75,000 - $99,999</td>
      <td>155</td>
      <td>321528.0</td>
      <td>12.040</td>
      <td>9.880</td>
      <td>14.200</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Income</td>
      <td>$100,000+</td>
      <td>213</td>
      <td>541245.0</td>
      <td>9.890</td>
      <td>8.340</td>
      <td>11.440</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2013</td>
      <td>Age</td>
      <td>18 to 34</td>
      <td>241</td>
      <td>916614.0</td>
      <td>9.960</td>
      <td>8.530</td>
      <td>11.390</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Age</td>
      <td>35 to 44</td>
      <td>198</td>
      <td>538114.0</td>
      <td>10.420</td>
      <td>8.840</td>
      <td>11.990</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2013</td>
      <td>Age</td>
      <td>45 to 54</td>
      <td>347</td>
      <td>880554.0</td>
      <td>16.810</td>
      <td>14.850</td>
      <td>18.770</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2013</td>
      <td>Age</td>
      <td>55 to 64</td>
      <td>456</td>
      <td>741063.0</td>
      <td>18.390</td>
      <td>16.600</td>
      <td>20.180</td>
    </tr>
    <tr>
      <th>45</th>
      <td>2013</td>
      <td>Age</td>
      <td>65+ years</td>
      <td>447</td>
      <td>569139.0</td>
      <td>13.410</td>
      <td>12.080</td>
      <td>14.750</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Get age group rows in depression df to change
depression_change2 = depression.loc[depression['strata_type'].isin(["35 to 44", "45 to 54", "55 to 64"])]
depression_change2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Age</td>
      <td>35 to 44</td>
      <td>198</td>
      <td>538114.0</td>
      <td>10.42</td>
      <td>8.84</td>
      <td>11.99</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2013</td>
      <td>Age</td>
      <td>45 to 54</td>
      <td>347</td>
      <td>880554.0</td>
      <td>16.81</td>
      <td>14.85</td>
      <td>18.77</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2013</td>
      <td>Age</td>
      <td>55 to 64</td>
      <td>456</td>
      <td>741063.0</td>
      <td>18.39</td>
      <td>16.60</td>
      <td>20.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create new row that combines data (frequncy sum and average for other stats)
depression_change2.loc[45] = [2013, "Age", "35 to 64",
    depression_change2.loc[42,'frequency'] + depression_change2.loc[43,'frequency'] + depression_change2.loc[44,'frequency'],
    (depression_change2.loc[42,'weighted_frequency'] + depression_change2.loc[43,'weighted_frequency'] + depression_change2.loc[44,'weighted_frequency'])/3,
    (depression_change2.loc[42,'percent'] + depression_change2.loc[43,'percent'] + depression_change2.loc[43,'percent'])/3,
    (depression_change2.loc[42,'lower_95'] + depression_change2.loc[43,'lower_95'] + depression_change2.loc[44,'lower_95'])/3,
    (depression_change2.loc[42,'upper_95'] + depression_change2.loc[43,'upper_95'] + depression_change2.loc[44,'upper_95'])/3,
    ]
depression_change2
```

    C:\Users\Xero\anaconda\Anaconda3\envs\PythonData\lib\site-packages\ipykernel_launcher.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      import sys
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Age</td>
      <td>35 to 44</td>
      <td>198</td>
      <td>538114.000000</td>
      <td>10.42</td>
      <td>8.84</td>
      <td>11.99</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2013</td>
      <td>Age</td>
      <td>45 to 54</td>
      <td>347</td>
      <td>880554.000000</td>
      <td>16.81</td>
      <td>14.85</td>
      <td>18.77</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2013</td>
      <td>Age</td>
      <td>55 to 64</td>
      <td>456</td>
      <td>741063.000000</td>
      <td>18.39</td>
      <td>16.60</td>
      <td>20.18</td>
    </tr>
    <tr>
      <th>45</th>
      <td>2013</td>
      <td>Age</td>
      <td>35 to 64</td>
      <td>1001</td>
      <td>719910.333333</td>
      <td>14.68</td>
      <td>13.43</td>
      <td>16.98</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Remove old rows (42, 43, 44), and insert new row into table (add 42). Sort index. 
new_depression2 = new_depression.drop([42, 43, 44])
new_depression2.loc[42] = depression_change2.loc[45]
new_depression2 = new_depression2.sort_index()
new_depression2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23</th>
      <td>2013</td>
      <td>Total</td>
      <td>Total</td>
      <td>1689</td>
      <td>NaN</td>
      <td>13.080</td>
      <td>12.330</td>
      <td>13.820</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2013</td>
      <td>Sex</td>
      <td>Male</td>
      <td>539</td>
      <td>1.307668e+06</td>
      <td>9.530</td>
      <td>8.530</td>
      <td>10.520</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2013</td>
      <td>Sex</td>
      <td>Female</td>
      <td>1150</td>
      <td>2.337817e+06</td>
      <td>16.520</td>
      <td>15.420</td>
      <td>17.620</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>White</td>
      <td>1103</td>
      <td>1.979888e+06</td>
      <td>15.970</td>
      <td>14.910</td>
      <td>17.020</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Black</td>
      <td>93</td>
      <td>2.528710e+05</td>
      <td>15.460</td>
      <td>11.910</td>
      <td>19.020</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Hispanic</td>
      <td>403</td>
      <td>1.011594e+06</td>
      <td>10.960</td>
      <td>9.720</td>
      <td>12.200</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Asian/Other</td>
      <td>90</td>
      <td>2.005655e+05</td>
      <td>14.505</td>
      <td>8.915</td>
      <td>20.095</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2013</td>
      <td>Education</td>
      <td>No High School Diploma</td>
      <td>222</td>
      <td>5.103710e+05</td>
      <td>12.840</td>
      <td>10.870</td>
      <td>14.810</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2013</td>
      <td>Education</td>
      <td>High School Graduate or GED Certificate</td>
      <td>361</td>
      <td>8.419950e+05</td>
      <td>14.280</td>
      <td>12.500</td>
      <td>16.060</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2013</td>
      <td>Education</td>
      <td>Some College or Tech School</td>
      <td>504</td>
      <td>1.069791e+06</td>
      <td>15.370</td>
      <td>13.810</td>
      <td>16.930</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2013</td>
      <td>Education</td>
      <td>College Graduate or Post Grad</td>
      <td>583</td>
      <td>1.170903e+06</td>
      <td>11.450</td>
      <td>10.310</td>
      <td>12.590</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2013</td>
      <td>Income</td>
      <td>&lt; $20,000</td>
      <td>600</td>
      <td>1.286100e+06</td>
      <td>18.630</td>
      <td>16.890</td>
      <td>20.380</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2013</td>
      <td>Income</td>
      <td>$20,000 - $34,999</td>
      <td>283</td>
      <td>5.607390e+05</td>
      <td>14.570</td>
      <td>12.500</td>
      <td>16.640</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2013</td>
      <td>Income</td>
      <td>$35,000 - $49,999</td>
      <td>149</td>
      <td>3.056770e+05</td>
      <td>11.290</td>
      <td>8.890</td>
      <td>13.680</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013</td>
      <td>Income</td>
      <td>$50,000 - $74,999</td>
      <td>186</td>
      <td>3.949930e+05</td>
      <td>12.260</td>
      <td>10.170</td>
      <td>14.340</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2013</td>
      <td>Income</td>
      <td>$75,000 - $99,999</td>
      <td>155</td>
      <td>3.215280e+05</td>
      <td>12.040</td>
      <td>9.880</td>
      <td>14.200</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Income</td>
      <td>$100,000+</td>
      <td>213</td>
      <td>5.412450e+05</td>
      <td>9.890</td>
      <td>8.340</td>
      <td>11.440</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2013</td>
      <td>Age</td>
      <td>18 to 34</td>
      <td>241</td>
      <td>9.166140e+05</td>
      <td>9.960</td>
      <td>8.530</td>
      <td>11.390</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Age</td>
      <td>35 to 64</td>
      <td>1001</td>
      <td>7.199103e+05</td>
      <td>14.680</td>
      <td>13.430</td>
      <td>16.980</td>
    </tr>
    <tr>
      <th>45</th>
      <td>2013</td>
      <td>Age</td>
      <td>65+ years</td>
      <td>447</td>
      <td>5.691390e+05</td>
      <td>13.410</td>
      <td>12.080</td>
      <td>14.750</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Get income rows in depression df to change
depression_change3 = depression.loc[depression['strata_type'].isin(["< $20,000", "$20,000 - $34,999",
         "$35,000 - $49,999", "$50,000 - $74,999","$75,000 - $99,999","$100,000+"])]
depression_change3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>35</th>
      <td>2013</td>
      <td>Income</td>
      <td>&lt; $20,000</td>
      <td>600</td>
      <td>1286100.0</td>
      <td>18.63</td>
      <td>16.89</td>
      <td>20.38</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2013</td>
      <td>Income</td>
      <td>$20,000 - $34,999</td>
      <td>283</td>
      <td>560739.0</td>
      <td>14.57</td>
      <td>12.50</td>
      <td>16.64</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2013</td>
      <td>Income</td>
      <td>$35,000 - $49,999</td>
      <td>149</td>
      <td>305677.0</td>
      <td>11.29</td>
      <td>8.89</td>
      <td>13.68</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013</td>
      <td>Income</td>
      <td>$50,000 - $74,999</td>
      <td>186</td>
      <td>394993.0</td>
      <td>12.26</td>
      <td>10.17</td>
      <td>14.34</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2013</td>
      <td>Income</td>
      <td>$75,000 - $99,999</td>
      <td>155</td>
      <td>321528.0</td>
      <td>12.04</td>
      <td>9.88</td>
      <td>14.20</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Income</td>
      <td>$100,000+</td>
      <td>213</td>
      <td>541245.0</td>
      <td>9.89</td>
      <td>8.34</td>
      <td>11.44</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create new rows that combines data (frequncy sum and average for other stats)
# < $35,000 income group
depression_change3.loc[41] = [2013, "Income", "< $35,000",
    depression_change3.loc[35,'frequency'] + depression_change3.loc[36,'frequency'],
    (depression_change3.loc[35,'weighted_frequency'] + depression_change3.loc[36,'weighted_frequency'])/2,
    (depression_change3.loc[35,'percent'] + depression_change3.loc[36,'percent'])/2,
    (depression_change3.loc[35,'lower_95'] + depression_change3.loc[36,'lower_95'])/2,
    (depression_change3.loc[35,'upper_95'] + depression_change3.loc[36,'upper_95'])/2,
    ]

# $50,000+ income group
depression_change3.loc[42] = [2013, "Income", "$50,000+",
    depression_change3.loc[38,'frequency'] + depression_change3.loc[39,'frequency'] + depression_change3.loc[40,'frequency'],
    (depression_change3.loc[38,'weighted_frequency'] + depression_change3.loc[39,'weighted_frequency'] + depression_change3.loc[40,'weighted_frequency'])/3,
    (depression_change3.loc[38,'percent'] + depression_change3.loc[39,'percent'] + depression_change3.loc[40,'percent'])/3,
    (depression_change3.loc[38,'lower_95'] + depression_change3.loc[39,'lower_95'] + depression_change3.loc[40,'lower_95'])/3,
    (depression_change3.loc[38,'upper_95'] + depression_change3.loc[39,'upper_95'] + depression_change3.loc[40,'upper_95'])/3,
    ]

depression_change3
```

    C:\Users\Xero\anaconda\Anaconda3\envs\PythonData\lib\site-packages\ipykernel_launcher.py:8: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    C:\Users\Xero\anaconda\Anaconda3\envs\PythonData\lib\site-packages\ipykernel_launcher.py:17: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>35</th>
      <td>2013</td>
      <td>Income</td>
      <td>&lt; $20,000</td>
      <td>600</td>
      <td>1.286100e+06</td>
      <td>18.630000</td>
      <td>16.890000</td>
      <td>20.380000</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2013</td>
      <td>Income</td>
      <td>$20,000 - $34,999</td>
      <td>283</td>
      <td>5.607390e+05</td>
      <td>14.570000</td>
      <td>12.500000</td>
      <td>16.640000</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2013</td>
      <td>Income</td>
      <td>$35,000 - $49,999</td>
      <td>149</td>
      <td>3.056770e+05</td>
      <td>11.290000</td>
      <td>8.890000</td>
      <td>13.680000</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013</td>
      <td>Income</td>
      <td>$50,000 - $74,999</td>
      <td>186</td>
      <td>3.949930e+05</td>
      <td>12.260000</td>
      <td>10.170000</td>
      <td>14.340000</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2013</td>
      <td>Income</td>
      <td>$75,000 - $99,999</td>
      <td>155</td>
      <td>3.215280e+05</td>
      <td>12.040000</td>
      <td>9.880000</td>
      <td>14.200000</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2013</td>
      <td>Income</td>
      <td>$100,000+</td>
      <td>213</td>
      <td>5.412450e+05</td>
      <td>9.890000</td>
      <td>8.340000</td>
      <td>11.440000</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2013</td>
      <td>Income</td>
      <td>&lt; $35,000</td>
      <td>883</td>
      <td>9.234195e+05</td>
      <td>16.600000</td>
      <td>14.695000</td>
      <td>18.510000</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Income</td>
      <td>$50,000+</td>
      <td>554</td>
      <td>4.192553e+05</td>
      <td>11.396667</td>
      <td>9.463333</td>
      <td>13.326667</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Remove old rows (35, 35, 38, 39, 40), and insert new rows into table (add 35, 38). Sort index. 
new_depression3 = new_depression2.drop([35, 36, 38, 39, 40])
new_depression3.loc[35] = depression_change3.loc[41]
new_depression3.loc[38] = depression_change3.loc[42]
edited_depression = new_depression3.sort_index()
edited_depression
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23</th>
      <td>2013</td>
      <td>Total</td>
      <td>Total</td>
      <td>1689</td>
      <td>NaN</td>
      <td>13.080000</td>
      <td>12.330000</td>
      <td>13.820000</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2013</td>
      <td>Sex</td>
      <td>Male</td>
      <td>539</td>
      <td>1.307668e+06</td>
      <td>9.530000</td>
      <td>8.530000</td>
      <td>10.520000</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2013</td>
      <td>Sex</td>
      <td>Female</td>
      <td>1150</td>
      <td>2.337817e+06</td>
      <td>16.520000</td>
      <td>15.420000</td>
      <td>17.620000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>White</td>
      <td>1103</td>
      <td>1.979888e+06</td>
      <td>15.970000</td>
      <td>14.910000</td>
      <td>17.020000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Black</td>
      <td>93</td>
      <td>2.528710e+05</td>
      <td>15.460000</td>
      <td>11.910000</td>
      <td>19.020000</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Hispanic</td>
      <td>403</td>
      <td>1.011594e+06</td>
      <td>10.960000</td>
      <td>9.720000</td>
      <td>12.200000</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2013</td>
      <td>Race-Ethnicity</td>
      <td>Asian/Other</td>
      <td>90</td>
      <td>2.005655e+05</td>
      <td>14.505000</td>
      <td>8.915000</td>
      <td>20.095000</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2013</td>
      <td>Education</td>
      <td>No High School Diploma</td>
      <td>222</td>
      <td>5.103710e+05</td>
      <td>12.840000</td>
      <td>10.870000</td>
      <td>14.810000</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2013</td>
      <td>Education</td>
      <td>High School Graduate or GED Certificate</td>
      <td>361</td>
      <td>8.419950e+05</td>
      <td>14.280000</td>
      <td>12.500000</td>
      <td>16.060000</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2013</td>
      <td>Education</td>
      <td>Some College or Tech School</td>
      <td>504</td>
      <td>1.069791e+06</td>
      <td>15.370000</td>
      <td>13.810000</td>
      <td>16.930000</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2013</td>
      <td>Education</td>
      <td>College Graduate or Post Grad</td>
      <td>583</td>
      <td>1.170903e+06</td>
      <td>11.450000</td>
      <td>10.310000</td>
      <td>12.590000</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2013</td>
      <td>Income</td>
      <td>&lt; $35,000</td>
      <td>883</td>
      <td>9.234195e+05</td>
      <td>16.600000</td>
      <td>14.695000</td>
      <td>18.510000</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2013</td>
      <td>Income</td>
      <td>$35,000 - $49,999</td>
      <td>149</td>
      <td>3.056770e+05</td>
      <td>11.290000</td>
      <td>8.890000</td>
      <td>13.680000</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013</td>
      <td>Income</td>
      <td>$50,000+</td>
      <td>554</td>
      <td>4.192553e+05</td>
      <td>11.396667</td>
      <td>9.463333</td>
      <td>13.326667</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2013</td>
      <td>Age</td>
      <td>18 to 34</td>
      <td>241</td>
      <td>9.166140e+05</td>
      <td>9.960000</td>
      <td>8.530000</td>
      <td>11.390000</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013</td>
      <td>Age</td>
      <td>35 to 64</td>
      <td>1001</td>
      <td>7.199103e+05</td>
      <td>14.680000</td>
      <td>13.430000</td>
      <td>16.980000</td>
    </tr>
    <tr>
      <th>45</th>
      <td>2013</td>
      <td>Age</td>
      <td>65+ years</td>
      <td>447</td>
      <td>5.691390e+05</td>
      <td>13.410000</td>
      <td>12.080000</td>
      <td>14.750000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Rename education strata types for obesity df
edited_obesity = edited_obesity.replace(
    {"Less than High School" : "No High School Diploma", 
     "High School Graduate" : "High School Graduate or GED Certificate", 
     "Some College" : "Some College or Tech School",
     "College Graduate" : "College Graduate or Post Grad"})
edited_obesity.reset_index(drop=True, inplace=True)
edited_obesity
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>age_group</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>obesity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>All</td>
      <td>Total</td>
      <td>30.200000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Gender</td>
      <td>Male</td>
      <td>26.500000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Gender</td>
      <td>Female</td>
      <td>33.900000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>18 - 34</td>
      <td>25.850000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>35 - 64</td>
      <td>34.300000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Age</td>
      <td>65+</td>
      <td>25.200000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>White</td>
      <td>27.100000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>African American</td>
      <td>44.200000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>Latino</td>
      <td>34.000000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Ethnicity</td>
      <td>Asian/Other</td>
      <td>23.900000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>No High School Diploma</td>
      <td>31.900000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>High School Graduate or GED Certificate</td>
      <td>32.700000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>Some College or Tech School</td>
      <td>33.800000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Education</td>
      <td>College Graduate or Post Grad</td>
      <td>23.100000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>&lt; $35,000</td>
      <td>32.733333</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$35,000 - 49,999</td>
      <td>28.200000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Income</td>
      <td>$50,000+</td>
      <td>24.000000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Physical Activity</td>
      <td>Met Aerobic Recommendation</td>
      <td>27.500000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>Physical Activity</td>
      <td>Did Not Meet Aerobic Recommendation</td>
      <td>34.600000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Participant</td>
      <td>34.400000</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Likely Eligible, â¤ 130%</td>
      <td>30.000000</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2013</td>
      <td>Adult (18+)</td>
      <td>CalFresh Status, % FPL</td>
      <td>Not Eligible, &gt; 185%</td>
      <td>20.900000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Rename gender, income, age, and race/ethnicity strata types for depression df
edited_depression = edited_depression.replace(
    {"Sex" : "Gender", 
     "$35,000 - $49,999": "$35,000 - 49,999",
     "18 to 34" : "18 - 34",
     "35 to 64" : "35 - 64",
     "65+ years" : "65+",
     "Race-Ethnicity" : "Ethnicity",
     "Black" : "African American", 
     "Asian/Pacific Islander" : "Asian/Other", 
     "Hispanic" : "Latino"})
edited_depression.reset_index(drop=True, inplace=True)
edited_depression
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>study_year</th>
      <th>strata</th>
      <th>strata_type</th>
      <th>frequency</th>
      <th>weighted_frequency</th>
      <th>percent</th>
      <th>lower_95</th>
      <th>upper_95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013</td>
      <td>Total</td>
      <td>Total</td>
      <td>1689</td>
      <td>NaN</td>
      <td>13.080000</td>
      <td>12.330000</td>
      <td>13.820000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013</td>
      <td>Gender</td>
      <td>Male</td>
      <td>539</td>
      <td>1.307668e+06</td>
      <td>9.530000</td>
      <td>8.530000</td>
      <td>10.520000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013</td>
      <td>Gender</td>
      <td>Female</td>
      <td>1150</td>
      <td>2.337817e+06</td>
      <td>16.520000</td>
      <td>15.420000</td>
      <td>17.620000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013</td>
      <td>Ethnicity</td>
      <td>White</td>
      <td>1103</td>
      <td>1.979888e+06</td>
      <td>15.970000</td>
      <td>14.910000</td>
      <td>17.020000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>Ethnicity</td>
      <td>African American</td>
      <td>93</td>
      <td>2.528710e+05</td>
      <td>15.460000</td>
      <td>11.910000</td>
      <td>19.020000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013</td>
      <td>Ethnicity</td>
      <td>Latino</td>
      <td>403</td>
      <td>1.011594e+06</td>
      <td>10.960000</td>
      <td>9.720000</td>
      <td>12.200000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2013</td>
      <td>Ethnicity</td>
      <td>Asian/Other</td>
      <td>90</td>
      <td>2.005655e+05</td>
      <td>14.505000</td>
      <td>8.915000</td>
      <td>20.095000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2013</td>
      <td>Education</td>
      <td>No High School Diploma</td>
      <td>222</td>
      <td>5.103710e+05</td>
      <td>12.840000</td>
      <td>10.870000</td>
      <td>14.810000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2013</td>
      <td>Education</td>
      <td>High School Graduate or GED Certificate</td>
      <td>361</td>
      <td>8.419950e+05</td>
      <td>14.280000</td>
      <td>12.500000</td>
      <td>16.060000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2013</td>
      <td>Education</td>
      <td>Some College or Tech School</td>
      <td>504</td>
      <td>1.069791e+06</td>
      <td>15.370000</td>
      <td>13.810000</td>
      <td>16.930000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2013</td>
      <td>Education</td>
      <td>College Graduate or Post Grad</td>
      <td>583</td>
      <td>1.170903e+06</td>
      <td>11.450000</td>
      <td>10.310000</td>
      <td>12.590000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2013</td>
      <td>Income</td>
      <td>&lt; $35,000</td>
      <td>883</td>
      <td>9.234195e+05</td>
      <td>16.600000</td>
      <td>14.695000</td>
      <td>18.510000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2013</td>
      <td>Income</td>
      <td>$35,000 - 49,999</td>
      <td>149</td>
      <td>3.056770e+05</td>
      <td>11.290000</td>
      <td>8.890000</td>
      <td>13.680000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2013</td>
      <td>Income</td>
      <td>$50,000+</td>
      <td>554</td>
      <td>4.192553e+05</td>
      <td>11.396667</td>
      <td>9.463333</td>
      <td>13.326667</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2013</td>
      <td>Age</td>
      <td>18 - 34</td>
      <td>241</td>
      <td>9.166140e+05</td>
      <td>9.960000</td>
      <td>8.530000</td>
      <td>11.390000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2013</td>
      <td>Age</td>
      <td>35 - 64</td>
      <td>1001</td>
      <td>7.199103e+05</td>
      <td>14.680000</td>
      <td>13.430000</td>
      <td>16.980000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2013</td>
      <td>Age</td>
      <td>65+</td>
      <td>447</td>
      <td>5.691390e+05</td>
      <td>13.410000</td>
      <td>12.080000</td>
      <td>14.750000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Connect to database
rds_connection_string = "root:<password>@127.0.0.1/obesity_depression_db"
engine = create_engine(f'mysql://{rds_connection_string}')
```


```python
# Check table names
engine.table_names()
```




    ['depression', 'obesity']




```python
# Transfer edited depression data to depression table in database
edited_depression.to_sql(name='depression', con=engine, if_exists='append', index=False)
```


```python
# Transfer edited obesity data to obesity table in database
edited_obesity.to_sql(name='obesity', con=engine, if_exists='append', index=False)
```
