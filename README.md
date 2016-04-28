# Hadoop-Pig-Basics-part-2-education-project
##This is a brief look, question 1, at a separate project from the Hadoop class.##
> 
##Edit the original file and safe as a tab seperated .csv file##
* note top header rows and bottom footer rows must be deleted before converting.
* non-conforming characters need to be removed.  In this case, all the rows that have numbers need all commas and dollar signs removed.
> 
##Upload the edited .csv file##
* $ hadoop fs -mkdir education
* $ hadoop fs -mkdir education
* $ hadoop fs -put occupation2.csv education
* $ pig -x mapred;
> 
> ##List all occupations that are expected to grow by over 20% by 2022.##
* grunt> mychart = Load '/user/cloudera/education/occupation2.csv' USING PigStorage('\t') AS (title:chararray, codej:chararray, type:chararray, emp_2014:double, emp_2024:double, empchangenum:double, empchangeperc:double, selfempperc:double, jobopennings:double, wage:double, educ:chararray, workexp:chararray, jobtraining:chararray);
 
* grunt> mychart  = FILTER mychart  BY  emp_2024 is not null AND emp_2024 is not null and  (emp_2024- emp_2014)/ emp_2014  > .2 AND type == 'Line item'; 
  * #This can also be done individually or broken into parts#
  * grunt> mychart  = FILTER mychart  BY  emp_2014 is not null; 
  * grunt> mychart  = FILTER mychart  BY  emp_2024 is not null; 
  * grunt> mychart  = FILTER mychart  BY  (emp_2024- emp_2014)/ emp_2014  > .2; 
  * grunt> mychart  = FILTER mychart  BY  type == 'Line item';
  * grunt> dump;

##OR just a count of them, instead of dump;, do the following##
* grunt> mygroup = GROUP mychart ALL;
* grunt> mycount = FOREACH mygroup GENERATE COUNT(mychart);
* grunt> dump;
> count = 51

##OR for less detail and an easier read##
* grunt> finallist = FOREACH mychart GENERATE title, ((emp_2024- emp_2014)/ emp_2014*100)  as perc:double;
* dump;
