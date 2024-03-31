# No-Show-Appointment Investigation
## Table of content

   - [Introduction](Introduction)
   
   - [Data Wrangling](Data-Wrangling)
   
   - [Exploratory Data Analysis](Exploratory-Data-Analysis)
   
   - [Conclusion](Conclusion)

## Introduction
### Dataset Description

In this notebook, i will be working on no-show appointments, basically trying to figure the average age of patients that turn up for there medical appointments. this dataset consist of a collection of informations from a medical appointment from hospitals in Brazil. the dataset is provided in one format, .csv

In this project, we will be grouping the patients by gender and age range then compare it to the no show appointment. we want to know if the patients age has a role to play in the show and no show appointment, then we also check if a certain gender showed up more than the other, this will enable us know if gender has a way of affecting the show and no show appointment too

The columns consist of: AGE: Tells the ages of each patient in the data PATIENTID: stands as a unique identifier for each patient GENDER:Tells the gender of each individual NEIGHBOURHOOD:indiates the location of each hospital SCHOLARSHIP: indicates whether or not the patient is enrolled in a scholarship program (HYPERTENSION/DIABETES/ALCOHOLISM/HANDCAP/SMS_RECEIVED): factors to be considered NO-SHOW: tells if a patient showed up or not

### Tools
- Numpy
- Pandas
- Seaborn
- Matplotlib

#### First we import the necessary packages and also load the data

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
appointment = pd.read_csv('noshowappointments.csv')
appointment.head()
```

## Data Wrangling
#### Here we get the number of rows and columns in the data

```appointment.shape```

#### we have 110527 rows and 14 columns. meaning there are 110527 record of patients that booked for appointments with 14 different attributes
#### lets get our data information, this will help us know if we have any missing values

```appointment.info()```

#### heere we have been able to get the datatypes for each column,we can also see that we have no missing values, so lets start our data cleaning
#### first lets check for duplicates

```sum(appointment.duplicated())```

#### we have no duplicated info here. since there is no duplicate, lets get the descriptive statistics of our data to know what to clean

```appointment.describe(percentiles = [0.25, 0.50, 0.75, 0.95, 0.99]).round(3)```

#### in age, we have -1 which is not feasible, so we need to drop that row

```appointment.drop(appointment[appointment["Age"] == appointment["Age"].min()].index, inplace = True)
appointment["Age"].describe()
```

#### we have a clean data now
#### lets remove the colunms we wont be needing for this analysis

```appointment.drop(['AppointmentID','ScheduledDay', 'Neighbourhood'], axis = 1, inplace =True )
appointment.head()
```

## Exploratory Data Analysis
#### let us know how many male and female patients we have in our dataset and there age range
```appointment[appointment['Gender'] == 'M']```

```appointment[appointment['Gender'] == 'F']```
 
#### the table above shows the details of each patients. we have more females that booked for an appointment than male patients


#### lets look at the graphical representation of our data

#### lets get the number of patients that showed up and ones that didnt show up according to gender
```
No_show_by_Gender = appointment.groupby(['Gender'])['No-show'].value_counts()
No_show_by_Gender
```

```appointment.groupby(['Gender'])['No-show'].value_counts().plot(kind = 'bar', figsize = (10,6)).set_ylabel('value_counts')```

From the chart above, more female gender showed up for there appointment than male, but going by the total number of people that booked for an appointment according to gender, we can see that females where obviously higher than males. so we cant say that gender is one of the contributing factor to the show and no show appointment

#### lets get the number of patients that showed up and ones that didnt show up according to age
```
No_show_by_age = appointment.groupby("No-show")["Age"].describe()
No_show_by_age
```


```
sns.set_style("dark")
appointment.boxplot(column= ["Age"], by=["No-show"], rot = 70)
plt.ylabel("Patient's Age")
plt.show()
```
88207 showed up for there appointment as against 22329 that didnt show up. we can also see that there is no much difference in the age specification because random ages came for there appointments.

#### lets get the number of patients that showed up and ones that didnt show up according to Alcoholism comparing it with age
```
appointment.groupby(['Scholarship'])['No-show'].value_counts()
```

```
sns.set_style("white")
sns.countplot(hue = 'No-show', x = 'Scholarship', data = appointment)
plt.ylabel('Number of patients for Scholarship')
plt.xlabel('Show or No show')
plt.title('A chat for Scholarship patients')
plt.show()
```
we can see that the patients with government funding is higher than those without, yet the turn up for the appointment was very small so we cant say that funding is a reason for show or no show appointment

#### lets get the number of patients that showed up and ones that didnt show up in total
```
total_of_no_show = appointment["No-show"].value_counts()
total_of_no_show
```

```
appointment['No-show'].value_counts().plot(kind='pie', figsize = (7,7))
plt.title("Total_of_no_show and show appointment", fontsize=50)
plt.show()
```
from the pie chat above, we can see that the number of patients that went for there appointment is quite higher than those that didnt go irrespective of there age, gender and other factors

## Conclusion
In conclusion, we have been able to look at why patients showed up and why some didnt using there age, gender and so on. From Scholarship, it turns out that people with scholarship are quite higher than those without, yet the ones with scholarship didnt turn up much. so we cant say that its because of funding that made them cancel for Age,its quite mixed up so the patients age has nothing to do with there turn up. same with Gender where we can see that more women registered than men yet the female turn up was low. Generally, we cant say that this factors in any way contributed to the patients turn up or no turn up

## Limitations
The arrangement of the colunm names was a bit of a difficulty as some have capitall letters inbetween. by arrangement, i mean the way its written. from the data provided we are unable to determine the reason for show and no show appointment so we cant arrive at a onclusion, i strongly believe that we have missing informations in the dataset. our No-show column was not stated in clearer terms











