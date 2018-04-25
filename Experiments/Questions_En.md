---
title : Practical Experiments of the Java Programing Language
---


[TOC]


#Basic Experiments

##1：Bisection method 

###Function :

$
f(x) = x^3 -10 x + 23 
$

###Initial values :

- $x_{low} = -10.0$
- $x_{high} = 5.0$
- $\delta = 0.001$

###Root-finding procedure :

1. $f(x_{high}) \cdot F(x_{low}) < 0$
2. $x_c = \frac{x_{high} + x_{low}}{2}$ 
3. if $f(x_c) = 0$，let $x_c$ as the result and exit the procedure
4. if $f(x_c) \neq 0$，go to $5$ or $6$
5. if $f(x_{high}) \cdot f(x_{c}) < 0$，$x_{low}= c$
6. if $f(x_{low}) \cdot f(x_{c}) < 0$，$x_{high}= c$
7. if $|x_{high}-x_{low}| \le \delta$，let $x_c$as the result and exit the procedure，else goto $2$。



##2：Find the largest prime number less than $10,000,000$

###Reference program：Finding all prime numbers less than $100$

~~~java
public class PrimeApp {
	public static void main(String[] args) {
		int m, n;// To determine whether the variable n is the prime number
		System.out.println("The prime numbers less than 100：");
		A: for (n = 2; n <= 100; n++) {
			for (m = 2; m < n / 2; m++) {
				if (n % m == 0)
					continue A;
             // If the variable n is divisible, out of the inner loop
			}

			System.out.print(n + " " + "\t");//output the prime number
		}
	}
}
~~~



##3：Implementation of the K-Means algorithm for the data clusering

<!---
- The basic idea of the K-Means algorithm is to randomly assign K cluster centers to classify the sample data by using the nearest neighbor principle.

- Recalculate the centers of each cluster, then update the new cluster centers.

- Iterates the above process until the movement distance of the cluster centers is less than a given value.
--->

###Procedure：

~~~
Randomly assign K cluster centers as the initial cluster centers

Repeat
  - Calculate the distance between each point in the data set and the center of each cluster
  - Assign all of the data point to the nearest cluster

  - Recalculate the centers of each cluster, then update the new cluster centers.
 
Until the movement distance of the cluster centers is less than a given value.
~~~

Where 

- K is the user-specified parameter, the number of clusters expected.
- Distance : Euclidean distance

###Data Sets:

- KMeans_Set.txt
- KMeans_Set2.txt



##4：Linear Regression

In statistics, linear regression is analysis method to find the relationship between one or more independent variables and dependent variables by using the least squares function.

###Principle

- Suppose the linear function:
$
f(x) = A x + B
$

- The training data set
$\{(x_k,y_k)\}_{k=1}^N $ 

- The mean square error
$E_2(f)^2 = \frac{1}{N} \sum_{k=1}^N (Ax_k + B - y_k)^2$

- If  $\frac{\partial E (A, B)}{\partial A} = 0$ and $\frac{\partial E (A, B)}{\partial B} = 0$

- Then
$
0 = \sum_{k=1}^N  (A x_k^2 + B x_k - x_k y_k) = A \sum_{k=1}^N x_k^2 + B \sum_{k=1}^N x_k - \sum_{k=1}^N x_k y_k
$
$
0 = \sum_{k=1}^N  (A x_k + B  -  y_k) = A \sum_{k=1}^N x_k + NB - \sum_{k=1}^N y_k
$

- Finally, the $A$和$B$ can be calculated by 
$
\left(  \sum_{k=1}^N x_k^2 \right) A + \left(  \sum_{k=1}^N x_k \right) B   = \sum_{k=1}^N x_k y_k 
$
$
\left(  \sum_{k=1}^N x_k \right) A + N B   =  \sum_{k=1}^N  y_k
$



###Data Sets:

- LR_ex0.txt
- LR_ex1.txt



##5：Student achievement statistics

###Requirements：

- Input the data from the specified file
- Get the highest score and the lowest score
- Calculate the average score for all students
- Get the score histogram between $60$～$69$，$70$～$79$，$80$～$89$ and $90～100$.

###Data Sets:

- score.cvs



#Optional Experiments

- Select at least three optional problems,

##6：Graphical user interface (GUI)

###Simulation of the simple calculator


###Gomoku game


 
##7：Database operation

###A simple information management system

To achieve the following functions:

- Create information table;
- query, modify, insert, delete the record;
- statistics of records;
- Import and export of recorded data (Excel file)

###E.g.

- Reference only, 
- The basic functions listed above must be completed.


####Student Information Management System

- Create a student table that contains the student's ID, name, and age information.
- According to the ID, you can check the student's name and age;
- Add a new record of a given student's  ID, name and age into the database;
- Remove the student's record from the database according to the student's ID;

#### Book information management system

- Create a book information table that contains the book title, book number, author, publication date information.
- According to the title, you can query the book number, author, publication date information;
- Add a new record of a given book's title, book number, author, publication date information into the database;
- Remove the book's record from the database according to the book's title



##8：Network programming

Implement a Java crawler

- Given a well-known website, such as Zhihu, Douban;
- Use the InetAddress class to get the IP address of the website;
- Get the name and IP address of the local machine;
- Use the URL class to download the webpage information,
- Filter and analysis the webpage, save the information as the text file.



##9：Coding and decoding of two-dimensional code

Using an open source QR library to implement a simple two-dimensional code encoding and decoding application.

The basic functions:

- Convert the input of a string or URL into a corresponding two-dimensional code picture;
- Parse the corresponding string or URL from an input 2D code image.

Reference:
<https://github.com/zxing/zxing>



##10 ：Duplicate file detection

The basic functions:

- set the the file directory;
- get the MD5 checksum value of each file while traversing the entire file directory;
- If the MD5 checksum values are same, compared the documents byte by byte to confirm whether the documents are the same;
- If the files are judged to be the same, the output prompts the user for the path and file name of the duplicate files.




#Requirements 

##Document fomat

- MS-Word
- PDF


##File name

>ID\_YourName.doc 

or

>ID\_YourName.pdf


##Content

### The report cover

### Each experimental topic 

- Experimental topics

- problem analysis

- the specific steps of the experiment

    Setting development environment, such as creating a database, choose Preferences third party open source libraries;

    Class and data structure design (UML);

    Code development process and testing (necessary screenshots)

- Complete code 

- experimental results and summary

    Results and the corresponding screenshot

    Analysis of the results
    
    Comment and discussion


---

You can get this document and datasets from Github ：
https://github.com/bushehui/Java_tutorial

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>




