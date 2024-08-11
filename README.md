# GROUP-BY-Extensions-in-PostgreSQL
Would you like to work smoothly and efficiently in PostgreSQL? Do you want to be able to create more complex queries? You need ROLLUP, CUBE and GROUPING SETS for this! Learn all about GROUP BY extension clauses in PostgreSQL.

**Introduction**
Welcome! This course will teach you how to use various **GROUP BY extensions: ROLLUP, CUBE, and GROUPING SETS.** They are often used to create advanced reports with PostgreSQL.

These extensions to the GROUP BY clause have been added to the ANSI/ISO standard SQL:1999, but they have been available in PostgreSQL since version 9.6.

We assume you already know much about SQL and can put that knowledge into practice. In particular, we assume that you know how to group rows by single or multiple columns and how to calculate aggregate functions: sums, averages, maximum values, etc.

**Get to know the tables – contest_score**
Let's first get to know the tables that we'll use in this part of the course.

Our examples will be based on a table named contest_score. You can check its contents using the button on the right side of the screen.

This table is used in a TV show in which contestants are given points for tasks in two categories: physical (Workout) and intellectual (Knowledge). scores from 1 to 10 are given over the course of five weeks to five contestants.

This table has the following columns: full_name, week, category, and score.

Exercise
SELECT * FROM contest_score

full_name	week	category	score
Dollie Esquivel	1	Workout	1
Aadil Castro	1	Workout	1
Aisling Keller	1	Workout	2
Lucie Muir	1	Workout	4
Fahmida Keith	1	Workout	6
Dollie Esquivel	2	Workout	8
Aadil Castro	2	Workout	7
Aisling Keller	2	Workout	10
Lucie Muir	2	Workout	8
Fahmida Keith	2	Workout	2
Dollie Esquivel	2	Workout	6
Aadil Castro	3	Workout	2
Aisling Keller	3	Workout	1
Lucie Muir	3	Workout	6
Fahmida Keith	3	Workout	2
Dollie Esquivel	4	Workout	7
Aadil Castro	4	Workout	6
Aisling Keller	4	Workout	2
Lucie Muir	4	Workout	1
Fahmida Keith	4	Workout	8
Dollie Esquivel	5	Workout	9
Aadil Castro	5	Workout	9
Aisling Keller	5	Workout	8
Lucie Muir	5	Workout	9
Fahmida Keith	5	Workout	4
Dollie Esquivel	1	Knowledge	9
Aadil Castro	1	Knowledge	9
Aisling Keller	1	Knowledge	10
Lucie Muir	1	Knowledge	5
Fahmida Keith	1	Knowledge	5
Dollie Esquivel	2	Knowledge	4
Aadil Castro	2	Knowledge	6
Aisling Keller	2	Knowledge	9
Lucie Muir	2	Knowledge	2
Fahmida Keith	2	Knowledge	1
Dollie Esquivel	2	Knowledge	2
Aadil Castro	3	Knowledge	3
Aisling Keller	3	Knowledge	4
Lucie Muir	3	Knowledge	9
Fahmida Keith	3	Knowledge	6
Dollie Esquivel	4	Knowledge	8
Aadil Castro	4	Knowledge	9
Aisling Keller	4	Knowledge	6
Lucie Muir	4	Knowledge	8
Fahmida Keith	4	Knowledge	4
Dollie Esquivel	5	Knowledge	6
Aadil Castro	5	Knowledge	4
Aisling Keller	5	Knowledge	7
Lucie Muir	5	Knowledge	6
Fahmida Keith	5	Knowledge	3

**Get to know the tables – delivery**
Great! The second table is named delivery; it will be used in our exercises. It is used by a supermarket to keep track of all deliveries.

The table has the following columns:

supplier (the company that delivers the products).
category (such as Toys or Office).
delivery_date (when the products were delivered to the store).
totalprice (how much was paid for the whole delivery).

SELECT * FROM delivery

supplier	category	delivery_date	total_price
ABCMacro	Groceries	2018-05-01	479.86
PyramidAv	Groceries	2018-05-01	605.73
Tulipfruit	Groceries	2018-05-02	747.32
Vineway	Groceries	2018-05-03	1.21
Acehead	Groceries	2018-05-04	174.78
Grasshopper Foods	Groceries	2018-05-04	953.43
Omegawalk	Office	2018-05-01	742.73
Yellowdew	Office	2018-05-01	6.73
Herolutions	Office	2018-05-02	419.61
Stormspace	Office	2018-05-03	835.90
Petaldale	Office	2018-05-04	652.01
DailyMilk	Dairy	2018-05-01	490.72
Robin	Dairy	2018-05-02	128.38
Mermaidland	Dairy	2018-05-03	672.51
Cubrews	Dairy	2018-05-04	965.94
Morning Thing	Dairy	2018-05-04	698.41
Joy Solutions	Toys	2018-05-01	228.23
Firetechs	Toys	2018-05-02	107.04
Beemaster	Toys	2018-05-03	340.54
Yewworth	Toys	2018-05-04	896.24
FullEnergy	Toys	2018-05-04	819.94

**Grouping by a single column**
To quickly recap what you already know about grouping rows in SQL, here is a simple problem: suppose we want to know the average score for each TV show contestant over the course of five weeks. In SQL, we'd write the following query:

SELECT
  full_name,
  AVG(score) AS avg_score
FROM contest_score
GROUP BY full_name;
In this simple query, we grouped the results by a single column (full_name) and used the AVG() function to calculate the average score for each contestant. Nothing really difficult.

Exercise
How much, in total, was spent on deliveries for each category? Show two columns: category and the sum of total_price (as total).

SELECT
	category,
	SUM(total_price) as total
FROM delivery
GROUP BY category

category	total
Dairy	2955.96
Groceries	2962.33
Toys	2391.99
Office	2656.98

Grouping by multiple columns
Great! Okay, we know how well each contestant did in general, but we would also like to know how well they did on average in each category. To achieve that, we can add another column to the GROUP BY clause:

SELECT
  full_name,
  category, 
  AVG(score) AS avg_score
FROM contest_score
GROUP BY full_name, category;
With this addition, we will now get each contestant's average score in each category.

full_name	category	avg_score
Aisling Keller	Workout	4.6000000000000000
Dollie Esquivel	Workout	6.2000000000000000
Aadil Castro	Workout	5.0000000000000000
Fahmida Keith	Workout	4.4000000000000000
Lucie Muir	Knowledge	6.0000000000000000
Aadil Castro	Knowledge	6.2000000000000000
Fahmida Keith	Knowledge	3.8000000000000000
Aisling Keller	Knowledge	7.2000000000000000
Dollie Esquivel	Knowledge	5.8000000000000000
Lucie Muir	Workout	5.6000000000000000

Exercise
How much, in total, was spent on deliveries for each category on each day? Show three columns: category, delivery_date, and the sum of total_price (rename the column total).

category	delivery_date	total
Toys	2018-05-04	1716.18
Dairy	2018-05-02	128.38
Groceries	2018-05-02	747.32
Office	2018-05-04	652.01
Groceries	2018-05-04	1128.21
Toys	2018-05-03	340.54
Dairy	2018-05-03	672.51
Toys	2018-05-01	228.23
Groceries	2018-05-01	1085.59
Toys	2018-05-02	107.04
Dairy	2018-05-04	1664.35
Office	2018-05-03	835.90
Groceries	2018-05-03	1.21
Dairy	2018-05-01	490.72
Office	2018-05-02	419.61
Office	2018-05-01	749.46

**ROLLUP introduction**
Good! Now we know how well each contestant did in general (GROUP BY full_name), and also how well they did in each category (GROUP BY full_name, category). But note one thing: by adding the second column to the GROUP BY clause, we lost some information from the previous query. With the GROUP BY full_name, category clause, we know the average scores for each contestant in each category, but we are no longer able to check the overall average for each contestant.

That's where GROUP BY ROLLUP comes in handy. Take a look:

SELECT
  full_name,
  category, 
  AVG(score) AS avg_score
FROM contest_score
GROUP BY ROLLUP (full_name, category);
Note the change in the GROUP BY clause. We added the ROLLUP operator, followed by a pair of parentheses. Inside, we put full_name and category.

Let's see what changes when we use ROLLUP.

Exercise
Run the template query and note what happens.

Apart from averages by contestant and category, we can also see averages by contestant and an overall average across all contestants and categories.

full_name	category	avg_score
null	null	5.4800000000000000
Aisling Keller	Workout	4.6000000000000000
Dollie Esquivel	Workout	6.2000000000000000
Aadil Castro	Workout	5.0000000000000000
Fahmida Keith	Workout	4.4000000000000000
Lucie Muir	Knowledge	6.0000000000000000
Aadil Castro	Knowledge	6.2000000000000000
Fahmida Keith	Knowledge	3.8000000000000000
Aisling Keller	Knowledge	7.2000000000000000
Dollie Esquivel	Knowledge	5.8000000000000000
Lucie Muir	Workout	5.6000000000000000
Dollie Esquivel	null	6.0000000000000000
Aadil Castro	null	5.6000000000000000
Fahmida Keith	null	4.1000000000000000
Lucie Muir	null	5.8000000000000000
Aisling Keller	null	5.9000000000000000

ROLLUP introduction – exercise
Great! We see that ROLLUP added two new rows to the query result: overall averages for each contestant and a general overall average for all contestants:

LEAD

Now it's your turn to try ROLLUP with the delivery table.

Exercise
Show how much was spent for each category on each day, for each category in general, and for all days and all categories. Show the following columns: category, delivery_date, and the sum of total_price (rename the column total).

SELECT
	category,
    delivery_date,
	SUM(total_price) as total
FROM delivery
GROUP BY ROLLUP (category,delivery_date);

Category	delivery_date	total
null	null	10967.26
Toys	2018-05-04	1716.18
Dairy	2018-05-02	128.38
Groceries	2018-05-02	747.32
Office	2018-05-04	652.01
Groceries	2018-05-04	1128.21
Toys	2018-05-03	340.54
Dairy	2018-05-03	672.51
Toys	2018-05-01	228.23
Groceries	2018-05-01	1085.59
Toys	2018-05-02	107.04
Dairy	2018-05-04	1664.35
Office	2018-05-03	835.90
Groceries	2018-05-03	1.21
Dairy	2018-05-01	490.72
Office	2018-05-02	419.61
Office	2018-05-01	749.46
Dairy	null	2955.96
Groceries	null	2962.33
Toys	null	2391.99
Office	null	2656.98

Order of columns in ROLLUP
Excellent. Now, you may have noticed that when we wrote:

SELECT
  full_name,
  category, 
  AVG(score) AS avg_score
FROM contest_score
GROUP BY ROLLUP (full_name, category);
we got the following groupings:

Average by full_name and category
Average by full_name
Overall average
I**n other words, we saw the full_name average, but we didn't see an average for category only. That's because the column order matters in ROLLUP.** Let's check that out.

Exercise
Run the template query. Note that the order of columns inside the parentheses of ROLLUP has been reversed.

As you can see, we now have the following groupings:

Average by full_name and category
Average by category
Overall average

SELECT
  full_name,
  category, 
  AVG(score) AS avg_score
FROM contest_score
GROUP BY ROLLUP (category, full_name);

full_name	category	avg_score
null	null	5.4800000000000000
Dollie Esquivel	Workout	6.2000000000000000
Aadil Castro	Workout	5.0000000000000000
Aisling Keller	Workout	4.6000000000000000
Aadil Castro	Knowledge	6.2000000000000000
Fahmida Keith	Knowledge	3.8000000000000000
Aisling Keller	Knowledge	7.2000000000000000
Dollie Esquivel	Knowledge	5.8000000000000000
Fahmida Keith	Workout	4.4000000000000000
Lucie Muir	Knowledge	6.0000000000000000
Lucie Muir	Workout	5.6000000000000000
null	Knowledge	5.8000000000000000
null	Workout	5.1600000000000000

**Order of columns in ROLLUP – exercise
Perfect! You can see that by reversing the order of the columns inside the ROLLUP's parentheses, we changed one of the groupings.

As a general rule, ROLLUP will always show new grouping combinations by removing columns one by one, starting from the right:

GROUP BY ROLLUP (A, B, C) =
GROUP BY (A, B, C) +
GROUP BY (A, B) +
GROUP BY (A) +
GROUP BY ()**
Okay, it's your turn now!

Exercise
Show how much was spent in each category on each day, on each day in general, and in general among all days and categories. Show the following columns: category, delivery_date, and the sum of total_price (as total).

SELECT
	category,
    delivery_date,
	SUM(total_price) as total
FROM delivery
GROUP BY ROLLUP (delivery_date,category);

category	delivery_date	total
null	null	10967.26
Groceries	2018-05-04	1128.21
Toys	2018-05-03	340.54
Office	2018-05-02	419.61
Toys	2018-05-04	1716.18
Dairy	2018-05-01	490.72
Office	2018-05-04	652.01
Toys	2018-05-02	107.04
Dairy	2018-05-02	128.38
Office	2018-05-01	749.46
Dairy	2018-05-04	1664.35
Groceries	2018-05-03	1.21
Groceries	2018-05-02	747.32
Toys	2018-05-01	228.23
Groceries	2018-05-01	1085.59
Office	2018-05-03	835.90
Dairy	2018-05-03	672.51
null	2018-05-01	2554.00
null	2018-05-03	1850.16
null	2018-05-04	5160.75
null	2018-05-02	1402.35

**The GROUPING() function
When using multiple columns inside ROLLUP's parentheses, it's quite easy to get lost among the resulting rows. SQL offers a function that tells you if the column is included in the grouping: GROUPING().

The GROUPING() function takes one column as an argument. It returns a 0 if the column is used in the grouping and a 1 if it is not. Take a look:**

SELECT
  full_name,
  category, 
  week,
  AVG(score) AS avg_score,
  GROUPING(full_name) AS F,
  GROUPING(category) AS C, 
  GROUPING(week) AS W
FROM contest_score
GROUP BY 
  ROLLUP (full_name, category, week);
  
As you can see, we added three GROUPING() functions in our SELECT clause. Inside the parentheses, we put all columns from the parentheses of ROLLUP. Let's see what the result is.

Exercise
Run the template query. As you can see, integer numbers appear in the final columns, denoting the grouping level for a given row.

Full_name	category	week	avg_score	f	c	w
null	null	null	5.4800000000000000	1	1	1
Fahmida Keith	Workout	4	8.0000000000000000	0	0	0
Aisling Keller	Knowledge	1	10.0000000000000000	0	0	0
Aisling Keller	Workout	2	10.0000000000000000	0	0	0
Aadil Castro	Knowledge	4	9.0000000000000000	0	0	0
Lucie Muir	Workout	4	1.00000000000000000000	0	0	0
Dollie Esquivel	Knowledge	4	8.0000000000000000	0	0	0
Lucie Muir	Knowledge	2	2.0000000000000000	0	0	0
Fahmida Keith	Knowledge	1	5.0000000000000000	0	0	0
Aadil Castro	Workout	3	2.0000000000000000	0	0	0
Lucie Muir	Knowledge	5	6.0000000000000000	0	0	0
Lucie Muir	Knowledge	1	5.0000000000000000	0	0	0
Aisling Keller	Workout	5	8.0000000000000000	0	0	0
Aadil Castro	Workout	4	6.0000000000000000	0	0	0
Fahmida Keith	Knowledge	3	6.0000000000000000	0	0	0
Lucie Muir	Workout	1	4.0000000000000000	0	0	0
Fahmida Keith	Workout	3	2.0000000000000000	0	0	0
Lucie Muir	Knowledge	3	9.0000000000000000	0	0	0
Fahmida Keith	Knowledge	2	1.00000000000000000000	0	0	0
Fahmida Keith	Workout	2	2.0000000000000000	0	0	0
Fahmida Keith	Knowledge	4	4.0000000000000000	0	0	0
Aadil Castro	Workout	5	9.0000000000000000	0	0	0
Dollie Esquivel	Knowledge	5	6.0000000000000000	0	0	0
Lucie Muir	Workout	3	6.0000000000000000	0	0	0
Dollie Esquivel	Workout	1	1.00000000000000000000	0	0	0
Fahmida Keith	Knowledge	5	3.0000000000000000	0	0	0
Lucie Muir	Knowledge	4	8.0000000000000000	0	0	0
Aadil Castro	Knowledge	3	3.0000000000000000	0	0	0
Dollie Esquivel	Knowledge	2	3.0000000000000000	0	0	0
Aadil Castro	Knowledge	2	6.0000000000000000	0	0	0
Aadil Castro	Workout	2	7.0000000000000000	0	0	0
Aisling Keller	Knowledge	5	7.0000000000000000	0	0	0
Fahmida Keith	Workout	1	6.0000000000000000	0	0	0
Aisling Keller	Workout	3	1.00000000000000000000	0	0	0
Lucie Muir	Workout	2	8.0000000000000000	0	0	0
Aisling Keller	Knowledge	4	6.0000000000000000	0	0	0
Dollie Esquivel	Workout	5	9.0000000000000000	0	0	0
Aisling Keller	Workout	1	2.0000000000000000	0	0	0
Lucie Muir	Workout	5	9.0000000000000000	0	0	0
Aisling Keller	Knowledge	3	4.0000000000000000	0	0	0
Aisling Keller	Knowledge	2	9.0000000000000000	0	0	0
Dollie Esquivel	Workout	2	7.0000000000000000	0	0	0
Aadil Castro	Knowledge	1	9.0000000000000000	0	0	0
Aadil Castro	Knowledge	5	4.0000000000000000	0	0	0
Fahmida Keith	Workout	5	4.0000000000000000	0	0	0
Dollie Esquivel	Knowledge	1	9.0000000000000000	0	0	0
Dollie Esquivel	Workout	4	7.0000000000000000	0	0	0
Aisling Keller	Workout	4	2.0000000000000000	0	0	0
Aadil Castro	Workout	1	1.00000000000000000000	0	0	0
Aisling Keller	Workout	null	4.6000000000000000	0	0	1
Dollie Esquivel	Workout	null	6.2000000000000000	0	0	1
Aadil Castro	Workout	null	5.0000000000000000	0	0	1
Fahmida Keith	Workout	null	4.4000000000000000	0	0	1
Lucie Muir	Knowledge	null	6.0000000000000000	0	0	1
Aadil Castro	Knowledge	null	6.2000000000000000	0	0	1
Fahmida Keith	Knowledge	null	3.8000000000000000	0	0	1
Aisling Keller	Knowledge	null	7.2000000000000000	0	0	1
Dollie Esquivel	Knowledge	null	5.8000000000000000	0	0	1
Lucie Muir	Workout	null	5.6000000000000000	0	0	1
Dollie Esquivel	null	null	6.0000000000000000	0	1	1
Aadil Castro	null	null	5.6000000000000000	0	1	1
Fahmida Keith	null	null	4.1000000000000000	0	1	1
Lucie Muir	null	null	5.8000000000000000	0	1	1
Aisling Keller	null	null	5.9000000000000000	0	1	1

Exercise
Show how much was spent, on average: for each supplier, in each category, for each supplier in general; and in general among all suppliers and categories.

Show the following columns: supplier, category, the average total_price (rename the column to avg_price), and two new columns (S and C) denoting whether the columns supplier or category are used in the grouping (0 if used, 1 otherwise).

SELECT
	supplier,
	category,
    AVG(total_price) as avg_price,
    GROUPING(supplier) AS S,
  	GROUPING(category) AS C 
  --GROUPING(week) AS W
FROM delivery
GROUP BY ROLLUP (supplier,category);

supplier	category	avg_price	s	c
null	null	522.2504761904761905	1	1
Grasshopper Foods	Groceries	953.4300000000000000	0	0
Mermaidland	Dairy	672.5100000000000000	0	0
FullEnergy	Toys	819.9400000000000000	0	0
Firetechs	Toys	107.0400000000000000	0	0
Cubrews	Dairy	965.9400000000000000	0	0
Beemaster	Toys	340.5400000000000000	0	0
Omegawalk	Office	742.7300000000000000	0	0
PyramidAv	Groceries	605.7300000000000000	0	0
Joy Solutions	Toys	228.2300000000000000	0	0
Yewworth	Toys	896.2400000000000000	0	0
Acehead	Groceries	174.7800000000000000	0	0
Morning Thing	Dairy	698.4100000000000000	0	0
DailyMilk	Dairy	490.7200000000000000	0	0
Petaldale	Office	652.0100000000000000	0	0
Robin	Dairy	128.3800000000000000	0	0
Tulipfruit	Groceries	747.3200000000000000	0	0
Stormspace	Office	835.9000000000000000	0	0
Vineway	Groceries	1.21000000000000000000	0	0
ABCMacro	Groceries	479.8600000000000000	0	0
Herolutions	Office	419.6100000000000000	0	0
Yellowdew	Office	6.7300000000000000	0	0
Yellowdew	null	6.7300000000000000	0	1
PyramidAv	null	605.7300000000000000	0	1
FullEnergy	null	819.9400000000000000	0	1
Tulipfruit	null	747.3200000000000000	0	1
DailyMilk	null	490.7200000000000000	0	1
Mermaidland	null	672.5100000000000000	0	1
Vineway	null	1.21000000000000000000	0	1
Grasshopper Foods	null	953.4300000000000000	0	1
Morning Thing	null	698.4100000000000000	0	1
Beemaster	null	340.5400000000000000	0	1
ABCMacro	null	479.8600000000000000	0	1
Stormspace	null	835.9000000000000000	0	1
Cubrews	null	965.9400000000000000	0	1
Acehead	null	174.7800000000000000	0	1
Omegawalk	null	742.7300000000000000	0	1
Robin	null	128.3800000000000000	0	1
Joy Solutions	null	228.2300000000000000	0	1
Petaldale	null	652.0100000000000000	0	1
Firetechs	null	107.0400000000000000	0	1
Herolutions	null	419.6100000000000000	0	1
Yewworth	null	896.2400000000000000	0	1

**Columns outside ROLLUP
Well done! Another thing you may be wondering about is whether you need to include all grouping columns inside the ROLLUP parentheses. No, you don't! You can leave some columns outside ROLLUP:**

SELECT
  full_name,
  category, 
  week,
  AVG(score) AS avg_price
FROM contest_score
GROUP BY 
  **ROLLUP (full_name, category)**,
  week;
**In the query above, all rows will be grouped by columns not included in ROLLUP. This means that the following grouping levels will be applied:

GROUP BY full_name, category, week
GROUP BY full_name, week
GROUP BY week**
Exercise
Run the template query and see what happens.

full_name	category	week	avg_price
Aisling Keller	Knowledge	3	4.0000000000000000
Dollie Esquivel	Knowledge	1	9.0000000000000000
Aisling Keller	Workout	5	8.0000000000000000
Lucie Muir	Workout	4	1.00000000000000000000
Lucie Muir	Knowledge	5	6.0000000000000000
Fahmida Keith	Workout	2	2.0000000000000000
Aisling Keller	Workout	2	10.0000000000000000
Fahmida Keith	Knowledge	4	4.0000000000000000
Lucie Muir	Workout	1	4.0000000000000000
Lucie Muir	Knowledge	1	5.0000000000000000
Aadil Castro	Knowledge	5	4.0000000000000000
Aadil Castro	Knowledge	4	9.0000000000000000
Fahmida Keith	Knowledge	1	5.0000000000000000
Aisling Keller	Knowledge	5	7.0000000000000000
Dollie Esquivel	Knowledge	4	8.0000000000000000
Lucie Muir	Knowledge	2	2.0000000000000000
Aadil Castro	Knowledge	3	3.0000000000000000
Aisling Keller	Workout	3	1.00000000000000000000
Dollie Esquivel	Workout	4	7.0000000000000000
Aadil Castro	Knowledge	2	6.0000000000000000
Aadil Castro	Knowledge	1	9.0000000000000000
Fahmida Keith	Knowledge	2	1.00000000000000000000
Lucie Muir	Knowledge	3	9.0000000000000000
Fahmida Keith	Knowledge	5	3.0000000000000000
Lucie Muir	Workout	5	9.0000000000000000
Fahmida Keith	Workout	4	8.0000000000000000
Fahmida Keith	Knowledge	3	6.0000000000000000
Aisling Keller	Knowledge	1	10.0000000000000000
Aisling Keller	Workout	1	2.0000000000000000
Lucie Muir	Workout	2	8.0000000000000000
Aadil Castro	Workout	2	7.0000000000000000
Dollie Esquivel	Knowledge	2	3.0000000000000000
Fahmida Keith	Workout	3	2.0000000000000000
Lucie Muir	Workout	3	6.0000000000000000
Aisling Keller	Knowledge	4	6.0000000000000000
Aisling Keller	Knowledge	2	9.0000000000000000
Aadil Castro	Workout	4	6.0000000000000000
Aadil Castro	Workout	1	1.00000000000000000000
Aisling Keller	Workout	4	2.0000000000000000
Fahmida Keith	Workout	5	4.0000000000000000
Dollie Esquivel	Workout	2	7.0000000000000000
Dollie Esquivel	Knowledge	5	6.0000000000000000
Fahmida Keith	Workout	1	6.0000000000000000
Dollie Esquivel	Workout	5	9.0000000000000000
Aadil Castro	Workout	3	2.0000000000000000
Aadil Castro	Workout	5	9.0000000000000000
Lucie Muir	Knowledge	4	8.0000000000000000
Dollie Esquivel	Workout	1	1.00000000000000000000
Aisling Keller	null	3	2.5000000000000000
Aadil Castro	null	5	6.5000000000000000
Fahmida Keith	null	1	5.5000000000000000
Aisling Keller	null	1	6.0000000000000000
Aadil Castro	null	3	2.5000000000000000
Aadil Castro	null	1	5.0000000000000000
Dollie Esquivel	null	1	5.0000000000000000
Fahmida Keith	null	3	4.0000000000000000
Fahmida Keith	null	4	6.0000000000000000
Dollie Esquivel	null	5	7.5000000000000000
Fahmida Keith	null	2	1.5000000000000000
Lucie Muir	null	3	7.5000000000000000
Lucie Muir	null	2	5.0000000000000000
Dollie Esquivel	null	4	7.5000000000000000
Dollie Esquivel	null	2	5.0000000000000000
Aisling Keller	null	4	4.0000000000000000
Aadil Castro	null	4	7.5000000000000000
Aadil Castro	null	2	6.5000000000000000
Fahmida Keith	null	5	3.5000000000000000
Lucie Muir	null	1	4.5000000000000000
Aisling Keller	null	5	7.5000000000000000
Lucie Muir	null	4	4.5000000000000000
Aisling Keller	null	2	9.5000000000000000
Lucie Muir	null	5	7.5000000000000000
null	null	3	4.1250000000000000
null	null	5	6.5000000000000000
null	null	4	5.9000000000000000
null	null	2	5.4166666666666667
null	null	1	5.2000000000000000

Exercise
Show how much was spent on average for each category on each day and on each day in general. Show the following columns: category, delivery_date, and the average total_price (name the column avg_price). Do not show a single general average among all days and categories.

SELECT
	category,
    delivery_date,
    AVG(total_price) as avg_price
    -- GROUPING(supplier) AS S,
  	-- GROUPING(category) AS C 
FROM delivery
GROUP BY ROLLUP (category),delivery_date;

category	delivery_date	avg_price
Groceries	2018-05-04	564.1050000000000000
Toys	2018-05-03	340.5400000000000000
Office	2018-05-02	419.6100000000000000
Toys	2018-05-04	858.0900000000000000
Dairy	2018-05-01	490.7200000000000000
Office	2018-05-04	652.0100000000000000
Toys	2018-05-02	107.0400000000000000
Dairy	2018-05-02	128.3800000000000000
Office	2018-05-01	374.7300000000000000
Dairy	2018-05-04	832.1750000000000000
Groceries	2018-05-03	1.21000000000000000000
Groceries	2018-05-02	747.3200000000000000
Toys	2018-05-01	228.2300000000000000
Groceries	2018-05-01	542.7950000000000000
Office	2018-05-03	835.9000000000000000
Dairy	2018-05-03	672.5100000000000000
null	2018-05-01	425.6666666666666667
null	2018-05-03	462.5400000000000000
null	2018-05-04	737.2500000000000000
null	2018-05-02	350.5875000000000000

Just for my trial adding grouping sets
SELECT
	category,
    delivery_date,
    AVG(total_price) as avg_price,
    GROUPING(category) AS C,
  	GROUPING(delivery_date) AS D 
FROM delivery
GROUP BY ROLLUP (category,delivery_date);

category	delivery_date	avg_price	c	d
null	null	522.2504761904761905	1	1
Toys	2018-05-04	858.0900000000000000	0	0
Dairy	2018-05-02	128.3800000000000000	0	0
Groceries	2018-05-02	747.3200000000000000	0	0
Office	2018-05-04	652.0100000000000000	0	0
Groceries	2018-05-04	564.1050000000000000	0	0
Toys	2018-05-03	340.5400000000000000	0	0
Dairy	2018-05-03	672.5100000000000000	0	0
Toys	2018-05-01	228.2300000000000000	0	0
Groceries	2018-05-01	542.7950000000000000	0	0
Toys	2018-05-02	107.0400000000000000	0	0
Dairy	2018-05-04	832.1750000000000000	0	0
Office	2018-05-03	835.9000000000000000	0	0
Groceries	2018-05-03	1.21000000000000000000	0	0
Dairy	2018-05-01	490.7200000000000000	0	0
Office	2018-05-02	419.6100000000000000	0	0
Office	2018-05-01	374.7300000000000000	0	0
Dairy	null	591.1920000000000000	0	1
Groceries	null	493.7216666666666667	0	1
Toys	null	478.3980000000000000	0	1
Office	null	531.3960000000000000	0	1

**Multiple ROLLUPs**
**Nice! If you need even more fine-grained control over the grouping combinations, you can also use multiple ROLLUPs:**

SELECT
  full_name,
  category, 
  week,
  AVG(score) AS avg_score
FROM contest_score
**GROUP BY 
  ROLLUP(full_name, category), 
  ROLLUP(week);
The query above will create even more combinations than

ROLLUP(full_name, category, week)**
**because the grouping options of ROLLUP(full_name, category) and the grouping options of ROLLUP(week) are combined together.** Take a look at the comparison:
full_name	category	week	avg_score
null	null	null	5.4800000000000000
Fahmida Keith	Workout	4	8.0000000000000000
Aisling Keller	Knowledge	1	10.0000000000000000
Aisling Keller	Workout	2	10.0000000000000000
Aadil Castro	Knowledge	4	9.0000000000000000
Lucie Muir	Workout	4	1.00000000000000000000
Dollie Esquivel	Knowledge	4	8.0000000000000000
Lucie Muir	Knowledge	2	2.0000000000000000
Fahmida Keith	Knowledge	1	5.0000000000000000
Aadil Castro	Workout	3	2.0000000000000000
Lucie Muir	Knowledge	5	6.0000000000000000
Lucie Muir	Knowledge	1	5.0000000000000000
Aisling Keller	Workout	5	8.0000000000000000
Aadil Castro	Workout	4	6.0000000000000000
Fahmida Keith	Knowledge	3	6.0000000000000000
Lucie Muir	Workout	1	4.0000000000000000
Fahmida Keith	Workout	3	2.0000000000000000
Lucie Muir	Knowledge	3	9.0000000000000000
Fahmida Keith	Knowledge	2	1.00000000000000000000
Fahmida Keith	Workout	2	2.0000000000000000
Fahmida Keith	Knowledge	4	4.0000000000000000
Aadil Castro	Workout	5	9.0000000000000000
Dollie Esquivel	Knowledge	5	6.0000000000000000
Lucie Muir	Workout	3	6.0000000000000000
Dollie Esquivel	Workout	1	1.00000000000000000000
Fahmida Keith	Knowledge	5	3.0000000000000000
Lucie Muir	Knowledge	4	8.0000000000000000
Aadil Castro	Knowledge	3	3.0000000000000000
Dollie Esquivel	Knowledge	2	3.0000000000000000
Aadil Castro	Knowledge	2	6.0000000000000000
Aadil Castro	Workout	2	7.0000000000000000
Aisling Keller	Knowledge	5	7.0000000000000000
Fahmida Keith	Workout	1	6.0000000000000000
Aisling Keller	Workout	3	1.00000000000000000000
Lucie Muir	Workout	2	8.0000000000000000
Aisling Keller	Knowledge	4	6.0000000000000000
Dollie Esquivel	Workout	5	9.0000000000000000
Aisling Keller	Workout	1	2.0000000000000000
Lucie Muir	Workout	5	9.0000000000000000
Aisling Keller	Knowledge	3	4.0000000000000000
Aisling Keller	Knowledge	2	9.0000000000000000
Dollie Esquivel	Workout	2	7.0000000000000000
Aadil Castro	Knowledge	1	9.0000000000000000
Aadil Castro	Knowledge	5	4.0000000000000000
Fahmida Keith	Workout	5	4.0000000000000000
Dollie Esquivel	Knowledge	1	9.0000000000000000
Dollie Esquivel	Workout	4	7.0000000000000000
Aisling Keller	Workout	4	2.0000000000000000
Aadil Castro	Workout	1	1.00000000000000000000
Aisling Keller	Workout	null	4.6000000000000000
Dollie Esquivel	Workout	null	6.2000000000000000
Aadil Castro	Workout	null	5.0000000000000000
Fahmida Keith	Workout	null	4.4000000000000000
Lucie Muir	Knowledge	null	6.0000000000000000
Aadil Castro	Knowledge	null	6.2000000000000000
Fahmida Keith	Knowledge	null	3.8000000000000000
Aisling Keller	Knowledge	null	7.2000000000000000
Dollie Esquivel	Knowledge	null	5.8000000000000000
Lucie Muir	Workout	null	5.6000000000000000
Dollie Esquivel	null	null	6.0000000000000000
Aadil Castro	null	null	5.6000000000000000
Fahmida Keith	null	null	4.1000000000000000
Lucie Muir	null	null	5.8000000000000000
Aisling Keller	null	null	5.9000000000000000
Aisling Keller	null	3	2.5000000000000000
Aadil Castro	null	5	6.5000000000000000
Fahmida Keith	null	1	5.5000000000000000
Aisling Keller	null	1	6.0000000000000000
Aadil Castro	null	3	2.5000000000000000
Aadil Castro	null	1	5.0000000000000000
Dollie Esquivel	null	1	5.0000000000000000
Fahmida Keith	null	3	4.0000000000000000
Fahmida Keith	null	4	6.0000000000000000
Dollie Esquivel	null	5	7.5000000000000000
Fahmida Keith	null	2	1.5000000000000000
Lucie Muir	null	3	7.5000000000000000
Lucie Muir	null	2	5.0000000000000000
Dollie Esquivel	null	4	7.5000000000000000
Dollie Esquivel	null	2	5.0000000000000000
Aisling Keller	null	4	4.0000000000000000
Aadil Castro	null	4	7.5000000000000000
Aadil Castro	null	2	6.5000000000000000
Fahmida Keith	null	5	3.5000000000000000
Lucie Muir	null	1	4.5000000000000000
Aisling Keller	null	5	7.5000000000000000
Lucie Muir	null	4	4.5000000000000000
Aisling Keller	null	2	9.5000000000000000
Lucie Muir	null	5	7.5000000000000000
null	null	3	4.1250000000000000
null	null	5	6.5000000000000000
null	null	4	5.9000000000000000
null	null	2	5.4166666666666667
null	null	1	5.2000000000000000

Exercise
Show how much was spent on average:

in each category on each day.
in each category.
on each day.
in general.
Show the following columns: category, delivery_date, and the average total_price (as avg_price).

SELECT
	category,
    delivery_date,
    avg(total_price) as avg_price
FROM delivery
GROUP BY ROLLUP (category),
		ROLLUP(delivery_date)

  category	delivery_date	avg_price
null	null	522.2504761904761905
Toys	2018-05-04	858.0900000000000000
Dairy	2018-05-02	128.3800000000000000
Groceries	2018-05-02	747.3200000000000000
Office	2018-05-04	652.0100000000000000
Groceries	2018-05-04	564.1050000000000000
Toys	2018-05-03	340.5400000000000000
Dairy	2018-05-03	672.5100000000000000
Toys	2018-05-01	228.2300000000000000
Groceries	2018-05-01	542.7950000000000000
Toys	2018-05-02	107.0400000000000000
Dairy	2018-05-04	832.1750000000000000
Office	2018-05-03	835.9000000000000000
Groceries	2018-05-03	1.21000000000000000000
Dairy	2018-05-01	490.7200000000000000
Office	2018-05-02	419.6100000000000000
Office	2018-05-01	374.7300000000000000
Dairy	null	591.1920000000000000
Groceries	null	493.7216666666666667
Toys	null	478.3980000000000000
Office	null	531.3960000000000000
null	2018-05-01	425.6666666666666667
null	2018-05-03	462.5400000000000000
null	2018-05-04	737.2500000000000000
null	2018-05-02	350.5875000000000000

**ROLLUP with COALESCE()
Perfect! The last thing we'll show you in this part is how to get rid of those nasty NULL values in higher grouping levels. To that end, we'll use the COALESCE() function.**

SELECT
  **COALESCE(full_name, 'All Contestants') AS full_name,
  COALESCE(category, 'All Categories') AS category,**
  week,
  AVG(score) AS avg_score
FROM contest_score
GROUP BY ROLLUP(full_name, category), week;
**COALESCE() takes as many arguments as you wish and returns the first element that is not NULL. For instance,

COALESCE(full_name, 'All Contestants')
will produce either the respective full_name value or the string 'All Contestants' if full_name is NULL. Naturally, you can use any other text value instead of 'All Contestants'.**
Exercise
Run the template query and see how it works.
