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

full_name	category	week	avg_score
Aadil Castro	All Categories	1	5.0000000000000000
Aisling Keller	All Categories	1	6.0000000000000000
All Contestants	All Categories	1	5.2000000000000000
Dollie Esquivel	All Categories	1	5.0000000000000000
Fahmida Keith	All Categories	1	5.5000000000000000
Lucie Muir	All Categories	1	4.5000000000000000
Aadil Castro	Knowledge	1	9.0000000000000000
Aisling Keller	Knowledge	1	10.0000000000000000
Dollie Esquivel	Knowledge	1	9.0000000000000000
Fahmida Keith	Knowledge	1	5.0000000000000000
Lucie Muir	Knowledge	1	5.0000000000000000
Aadil Castro	Workout	1	1.00000000000000000000
Aisling Keller	Workout	1	2.0000000000000000
Dollie Esquivel	Workout	1	1.00000000000000000000
Fahmida Keith	Workout	1	6.0000000000000000
Lucie Muir	Workout	1	4.0000000000000000
Aadil Castro	All Categories	2	6.5000000000000000
Aisling Keller	All Categories	2	9.5000000000000000
All Contestants	All Categories	2	5.4166666666666667
Dollie Esquivel	All Categories	2	5.0000000000000000
Fahmida Keith	All Categories	2	1.5000000000000000
Lucie Muir	All Categories	2	5.0000000000000000
Aadil Castro	Knowledge	2	6.0000000000000000
Aisling Keller	Knowledge	2	9.0000000000000000
Dollie Esquivel	Knowledge	2	3.0000000000000000
Fahmida Keith	Knowledge	2	1.00000000000000000000
Lucie Muir	Knowledge	2	2.0000000000000000
Aadil Castro	Workout	2	7.0000000000000000
Aisling Keller	Workout	2	10.0000000000000000
Dollie Esquivel	Workout	2	7.0000000000000000
Fahmida Keith	Workout	2	2.0000000000000000
Lucie Muir	Workout	2	8.0000000000000000
Aadil Castro	All Categories	3	2.5000000000000000
Aisling Keller	All Categories	3	2.5000000000000000
All Contestants	All Categories	3	4.1250000000000000
Fahmida Keith	All Categories	3	4.0000000000000000
Lucie Muir	All Categories	3	7.5000000000000000
Aadil Castro	Knowledge	3	3.0000000000000000
Aisling Keller	Knowledge	3	4.0000000000000000
Fahmida Keith	Knowledge	3	6.0000000000000000
Lucie Muir	Knowledge	3	9.0000000000000000
Aadil Castro	Workout	3	2.0000000000000000
Aisling Keller	Workout	3	1.00000000000000000000
Fahmida Keith	Workout	3	2.0000000000000000
Lucie Muir	Workout	3	6.0000000000000000
Aadil Castro	All Categories	4	7.5000000000000000
Aisling Keller	All Categories	4	4.0000000000000000
All Contestants	All Categories	4	5.9000000000000000
Dollie Esquivel	All Categories	4	7.5000000000000000
Fahmida Keith	All Categories	4	6.0000000000000000
Lucie Muir	All Categories	4	4.5000000000000000
Aadil Castro	Knowledge	4	9.0000000000000000
Aisling Keller	Knowledge	4	6.0000000000000000
Dollie Esquivel	Knowledge	4	8.0000000000000000
Fahmida Keith	Knowledge	4	4.0000000000000000
Lucie Muir	Knowledge	4	8.0000000000000000
Aadil Castro	Workout	4	6.0000000000000000
Aisling Keller	Workout	4	2.0000000000000000
Dollie Esquivel	Workout	4	7.0000000000000000
Fahmida Keith	Workout	4	8.0000000000000000
Lucie Muir	Workout	4	1.00000000000000000000
Aadil Castro	All Categories	5	6.5000000000000000
Aisling Keller	All Categories	5	7.5000000000000000
All Contestants	All Categories	5	6.5000000000000000
Dollie Esquivel	All Categories	5	7.5000000000000000
Fahmida Keith	All Categories	5	3.5000000000000000
Lucie Muir	All Categories	5	7.5000000000000000
Aadil Castro	Knowledge	5	4.0000000000000000
Aisling Keller	Knowledge	5	7.0000000000000000
Dollie Esquivel	Knowledge	5	6.0000000000000000
Fahmida Keith	Knowledge	5	3.0000000000000000
Lucie Muir	Knowledge	5	6.0000000000000000
Aadil Castro	Workout	5	9.0000000000000000
Aisling Keller	Workout	5	8.0000000000000000
Dollie Esquivel	Workout	5	9.0000000000000000
Fahmida Keith	Workout	5	4.0000000000000000
Lucie Muir	Workout	5	9.0000000000000000

Exercise
Show the total price paid for all deliveries in each category and a grand total for all categories. Show the category column and the sum of the total_price. The columns names should be category and total. Instead of NULL values in the category column, show two hyphens (--).

SELECT
	COALESCE(category,'--') AS category,
    SUM(total_price) as total
FROM delivery
GROUP BY ROLLUP (category)

category	total
--	10967.26
Dairy	2955.96
Groceries	2962.33
Toys	2391.99
Office	2656.98

w/o Coalesce,
SELECT
	category,
    SUM(total_price) as total
FROM delivery
GROUP BY ROLLUP (category)

category	total
null	10967.26
Dairy	2955.96
Groceries	2962.33
Toys	2391.99
Office	2656.98

Summary
Alright, let's wrap things up!

GROUP BY ROLLUP() works by creating additional rows with fewer grouping columns, as shown below:
GROUP BY ROLLUP (A, B, C) =
GROUP BY (A, B, C) +
GROUP BY (A, B) +
GROUP BY (A) +
GROUP BY ()
By changing the order of columns inside ROLLUP, we change the grouping levels created.
Not all columns must be included inside ROLLUP. Those outside its parentheses will always be used for grouping.
We can use COALESCE(column_name, substitute_value) to show the substitute_value when column_name is NULL.
The GROUPING(column_name) function shows if the column_name column is used in the grouping.

Exercise 1
Great! For this quiz, we're going to use a simple table:

department_cost(department, year, total_cost)
It shows the overall cost of running a given department in a given year. The cost is expressed in thousands of dollars.

Exercise
Show the sum of the total_cost for each department and year; the total sum of total_cost for each department across all years; and the grand total of total_cost across all departments and years.

Show the following columns: department, year, and the sum of total_cost (as total).

SELECT
	department_cost.department,
    year,
    sum(total_cost) AS total
FROM department_cost
GROUP BY ROLLUP (department_cost.department,
    year)

    department	year	total
null	null	9156
Marketing	2017	645
IT	2017	558
IT	2016	586
Accounting	2017	442
Marketing	2015	720
Accounting	2018	599
Sales	2015	789
Sales	2016	554
Accounting	2015	305
Sales	2017	737
Marketing	2018	562
Marketing	2016	506
IT	2018	327
Sales	2018	685
Accounting	2016	550
IT	2015	591
Accounting	null	1896
Marketing	null	2433
Sales	null	2765
IT	null	2062

Exercise
Show the sum of the total_cost for each department and year, and the total sum of total_cost for each year. Do not show the grand total.

Show the following columns: department, year, and the sum of total_cost (as total). Instead of NULLs in the department column, show the following text: 'unknown'.

Make sure that your table consists of the following columns: department, year, and total.

SELECT
	COALESCE(department_cost.department,'unknown') As department,
    year,
    sum(total_cost) AS total
FROM department_cost
GROUP BY ROLLUP (department_cost.department),
	year;

 department	year	total
Sales	2016	554
Sales	2017	737
Marketing	2016	506
Accounting	2016	550
Accounting	2018	599
Marketing	2017	645
IT	2018	327
Marketing	2018	562
IT	2015	591
Sales	2015	789
Marketing	2015	720
Accounting	2015	305
IT	2016	586
IT	2017	558
Accounting	2017	442
Sales	2018	685
unknown	2017	2382
unknown	2016	2196
unknown	2018	2173
unknown	2015	2405
-------------------------------------------------------------------------------------------
2.
CUBE
Discover CUBE, another GROUP BY extension.

Introduction
Welcome to the second part of our course!

You learned how to use ROLLUP in the previous part. Here, we will practice CUBE – another GROUP BY extension that is mainly used in ETL processes (i.e., when working with data warehouses and creating advanced reports).

Shall we start?

Get to know the tables – vaccine_administration
Let’s get to know the tables we’ll be working with.

We will use a table named vaccine_administration in our examples. It describes vaccination efficacy in various test patients. The following columns are available:

id – A unique identifier for each record/row in the table.
location – A patient’s location, either Norway or Argentina.
gender – Denoted as either Male or Female.
risk – The patient’s risk of contracting a disease: Low, Medium, or High.
age – The patient’s age in years.
efficacy – An integer value ranging from 1 (poor vaccination efficacy) to 3 (excellent efficacy).
Exercise
Select all information from the vaccine_administration table.

id	location	gender	risk	age	efficacy
1	Norway	Male	Low	33	3
2	Norway	Male	Low	22	3
3	Norway	Male	Low	59	3
4	Norway	Male	Low	23	3
5	Norway	Male	Low	42	3
6	Norway	Male	Low	41	3
7	Norway	Male	Medium	24	2
8	Norway	Male	Medium	19	3
9	Norway	Male	Medium	46	3
10	Norway	Male	High	55	2
11	Norway	Male	High	57	2
12	Norway	Male	High	43	1
13	Norway	Female	Low	49	2
14	Norway	Female	Low	22	3
15	Norway	Female	Low	36	3
16	Norway	Female	Low	29	2
17	Norway	Female	Medium	23	3
18	Norway	Female	Medium	25	3
19	Norway	Female	Medium	32	3
20	Norway	Female	Medium	60	3
21	Norway	Female	High	53	1
22	Norway	Female	High	23	1
23	Norway	Female	High	31	2
24	Norway	Female	High	41	3
25	Argentina	Male	Low	52	1
26	Argentina	Male	Low	60	1
27	Argentina	Male	Medium	51	2
28	Argentina	Male	Medium	47	2
29	Argentina	Male	Medium	27	2
30	Argentina	Male	Medium	28	3
31	Argentina	Male	Medium	33	1
32	Argentina	Male	Medium	26	1
33	Argentina	Male	Medium	46	1
34	Argentina	Male	High	31	2
35	Argentina	Male	High	50	2
36	Argentina	Male	High	30	1
37	Argentina	Female	Low	23	2
38	Argentina	Female	Low	51	2
39	Argentina	Female	Low	52	3
40	Argentina	Female	Low	26	3
41	Argentina	Female	Medium	44	1
42	Argentina	Female	Medium	48	1
43	Argentina	Female	Medium	49	2
44	Argentina	Female	High	50	2
45	Argentina	Female	High	26	3
46	Argentina	Female	High	58	3
47	Argentina	Female	High	45	1
48	Argentina	Female	High	58	2
49	Argentina	Female	High	34	2
50	Argentina	Female	High	38	3

Get to know the tables – wildfire_incident
Now, let’s take a look at the table that you’ll be querying in the exercises. It is named wildfire_incident and it contains the following columns:

id – A unique identifier for each record/row in the table.
year – The year when a given wildfire started: 2016, 2017, or 2018.
month – The month when a given wildfire started: May, June, or July.
cause – Lightning Strike, Arson, Spontaneous Combustion, or Unintentional Human Involvement.
damage_repair_cost – estimated cost to repair the damage caused by a given wildfire.
duration – The number of hours a given wildfire lasted.
Exercise
Select all information from the wildfire_incident table.

id	year	month	cause	damage_repair_cost	duration
1	2016	May	Lightning Strike	352245	7
2	2016	May	Arson	761208	22
3	2016	May	Spontaneous Combustion	996581	27
4	2017	May	Unintentional Human Involvement	343963	21
5	2017	May	Arson	926046	17
6	2017	May	Lightning Strike	258575	17
7	2018	May	Spontaneous Combustion	473368	12
8	2018	May	Arson	538433	8
9	2018	May	Lightning Strike	722932	8
10	2016	June	Unintentional Human Involvement	885557	12
11	2016	June	Arson	721758	12
12	2016	June	Spontaneous Combustion	674498	12
13	2017	June	Unintentional Human Involvement	176915	6
14	2017	June	Spontaneous Combustion	513915	14
15	2017	June	Lightning Strike	746398	8
16	2018	June	Arson	161928	22
17	2018	June	Unintentional Human Involvement	587734	13
18	2018	June	Spontaneous Combustion	854244	4
19	2016	July	Lightning Strike	229822	9
20	2016	July	Unintentional Human Involvement	531561	23
21	2016	July	Spontaneous Combustion	490614	5
22	2017	July	Lightning Strike	725808	8
23	2017	July	Lightning Strike	722180	25
24	2017	July	Spontaneous Combustion	606780	16
25	2018	July	Spontaneous Combustion	813427	26
26	2018	July	Unintentional Human Involvement	561721	8
27	2018	July	Arson	637556	17

Introduction to CUBE
Let’s get started. In principle, ROLLUP and CUBE are similar. The difference is that CUBE does not remove columns from the right to create grouping levels. Instead, it creates every possible grouping combination based on the columns inside its parentheses. Take a look:

SELECT
  location, 
  gender, 
  risk, 
  AVG(age) AS avg_age
FROM vaccine_administration
GROUP BY CUBE (location, gender, risk);
Syntax-wise, we only replaced ROLLUP with CUBE. Let’s see what result we’ll get.

Exercise
Run the template query and note how many grouping levels are created.

location	gender	risk	avg_age
null	null	null	39.4200000000000000
Argentina	Female	Medium	47.0000000000000000
Norway	Male	Medium	29.6666666666666667
Argentina	Male	Low	56.0000000000000000
Argentina	Female	High	44.1428571428571429
Norway	Female	High	37.0000000000000000
Norway	Female	Low	34.0000000000000000
Argentina	Female	Low	38.0000000000000000
Norway	Male	Low	36.6666666666666667
Argentina	Male	High	37.0000000000000000
Argentina	Male	Medium	36.8571428571428571
Norway	Female	Medium	35.0000000000000000
Norway	Male	High	51.6666666666666667
Argentina	Female	null	43.0000000000000000
Argentina	Male	null	40.0833333333333333
Norway	Female	null	35.3333333333333333
Norway	Male	null	38.6666666666666667
Argentina	null	null	41.6538461538461538
Norway	null	null	37.0000000000000000
null	Female	Low	36.0000000000000000
null	Male	Medium	34.7000000000000000
null	Male	Low	41.5000000000000000
null	Female	Medium	40.1428571428571429
null	Male	High	44.3333333333333333
null	Female	High	41.5454545454545455
null	Female	null	39.4615384615384615
null	Male	null	39.3750000000000000
Norway	null	High	43.2857142857142857
Norway	null	Medium	32.7142857142857143
Argentina	null	Low	44.0000000000000000
Argentina	null	Medium	39.9000000000000000
Argentina	null	High	42.0000000000000000
Norway	null	Low	35.6000000000000000
null	null	Medium	36.9411764705882353
null	null	High	42.5294117647058824
null	null	Low	38.7500000000000000

ntroduction to CUBE – exercise
As you could see, the following rows were created:

GROUP BY CUBE(location, gender, risk) =
GROUP BY location, gender, risk +
GROUP BY location, gender +
GROUP BY gender, risk +
GROUP BY location, risk +
GROUP BY location +
GROUP BY gender +
GROUP BY risk +
GROUP BY ()
Quite a lot! For n columns passed in as parameters, CUBE creates 
2
𝑛
2 
n
  grouping levels, while ROLLUP only creates 
𝑛
+
1
n+1 levels:

No. columns	No. grouping levels
ROLLUP	CUBE
1	2	2
2	3	4
3	4	8
4	5	16
5	6	32
6	7	64
You need to be aware that CUBE can significantly lower the performance of your queries. As few as three columns in CUBE create eight different types of groupings. Even though a query with CUBE is faster than separate grouping queries merged with UNION, performance can still be an issue for large tables.

Exercise
Show the sum of damage_repair_cost (as sum_damage_repair_cost) for all possible grouping combinations based on the year, month, and cause columns.

Show the following columns in the query result: year, month, cause, and sum_damage_repair_cost.

SELECT
	year,
    month,
    cause,
    sum(damage_repair_cost)as sum_damage_repair_cost
FROM  wildfire_incident
GROUP BY CUBE(year,month,cause)

year	month	cause	sum_damage_repair_cost
null	null	null	16015767
2017	June	Lightning Strike	746398
2016	May	Arson	761208
2018	July	Unintentional Human Involvement	561721
2018	May	Spontaneous Combustion	473368
2017	May	Arson	926046
2018	June	Arson	161928
2017	July	Spontaneous Combustion	606780
2017	May	Lightning Strike	258575
2016	June	Spontaneous Combustion	674498
2018	June	Unintentional Human Involvement	587734
2016	July	Spontaneous Combustion	490614
2016	June	Unintentional Human Involvement	885557
2017	July	Lightning Strike	1447988
2018	May	Lightning Strike	722932
2016	June	Arson	721758
2016	May	Lightning Strike	352245
2018	June	Spontaneous Combustion	854244
2018	May	Arson	538433
2016	July	Lightning Strike	229822
2017	June	Spontaneous Combustion	513915
2018	July	Spontaneous Combustion	813427
2017	May	Unintentional Human Involvement	343963
2016	May	Spontaneous Combustion	996581
2016	July	Unintentional Human Involvement	531561
2018	July	Arson	637556
2017	June	Unintentional Human Involvement	176915
2017	July	null	2054768
2016	July	null	1251997
2017	June	null	1437228
2016	May	null	2110034
2018	May	null	1734733
2018	June	null	1603906
2017	May	null	1528584
2018	July	null	2012704
2016	June	null	2281813
2017	null	null	5020580
2016	null	null	5643844
2018	null	null	5351343
null	May	Arson	2225687
null	May	Spontaneous Combustion	1469949
null	June	Spontaneous Combustion	2042657
null	July	Arson	637556
null	July	Unintentional Human Involvement	1093282
null	July	Spontaneous Combustion	1910821
null	May	Unintentional Human Involvement	343963
null	June	Unintentional Human Involvement	1650206
null	July	Lightning Strike	1677810
null	June	Arson	883686
null	June	Lightning Strike	746398
null	May	Lightning Strike	1333752
null	May	null	5373351
null	July	null	5319469
null	June	null	5322947
2017	null	Spontaneous Combustion	1120695
2017	null	Arson	926046
2018	null	Spontaneous Combustion	2141039
2017	null	Unintentional Human Involvement	520878
2016	null	Unintentional Human Involvement	1417118
2018	null	Arson	1337917
2018	null	Lightning Strike	722932
2017	null	Lightning Strike	2452961
2016	null	Spontaneous Combustion	2161693
2018	null	Unintentional Human Involvement	1149455
2016	null	Lightning Strike	582067
2016	null	Arson	1482966
null	null	Lightning Strike	3757960
null	null	Arson	3746929
null	null	Spontaneous Combustion	5423427
null	null	Unintentional Human Involvement	3087451

Order of Columns in CUBE
Good job! CUBE creates all possible grouping combinations. This means that, unlike ROLLUP, CUBE does not change the query result when the order of columns inside the parentheses is changed.

Let’s verify this with a practice exercise.

Exercise
Use the template to experiment with various column orders. 

SELECT
  year,
  month,
  cause,
  SUM(damage_repair_cost) As sum_damage_repair_cost
FROM wildfire_incident
GROUP BY CUBE (month,year, cause);
year	month	cause	sum_damage_repair_cost
null	null	null	16015767
2017	May	Lightning Strike	258575
2016	July	Unintentional Human Involvement	531561
2018	July	Spontaneous Combustion	813427
2017	June	Lightning Strike	746398
2017	June	Spontaneous Combustion	513915
2016	May	Arson	761208
2016	May	Spontaneous Combustion	996581
2017	July	Spontaneous Combustion	606780
2018	June	Arson	161928
2018	June	Unintentional Human Involvement	587734
2018	May	Arson	538433
2016	July	Spontaneous Combustion	490614
2016	May	Lightning Strike	352245
2018	July	Arson	637556
2017	May	Unintentional Human Involvement	343963
2016	June	Spontaneous Combustion	674498
2018	July	Unintentional Human Involvement	561721
2018	May	Lightning Strike	722932
2018	May	Spontaneous Combustion	473368
2016	June	Unintentional Human Involvement	885557
2016	July	Lightning Strike	229822
2017	July	Lightning Strike	1447988
2017	June	Unintentional Human Involvement	176915
2017	May	Arson	926046
2016	June	Arson	721758
2018	June	Spontaneous Combustion	854244
2017	July	null	2054768
2017	June	null	1437228
2016	May	null	2110034
2018	June	null	1603906
2016	June	null	2281813
2017	May	null	1528584
2018	July	null	2012704
2016	July	null	1251997
2018	May	null	1734733
null	May	null	5373351
null	July	null	5319469
null	June	null	5322947
2016	null	Lightning Strike	582067
2017	null	Arson	926046
2017	null	Spontaneous Combustion	1120695
2017	null	Lightning Strike	2452961
2018	null	Lightning Strike	722932
2018	null	Unintentional Human Involvement	1149455
2018	null	Spontaneous Combustion	2141039
2016	null	Spontaneous Combustion	2161693
2016	null	Unintentional Human Involvement	1417118
2016	null	Arson	1482966
2018	null	Arson	1337917
2017	null	Unintentional Human Involvement	520878
2017	null	null	5020580
2016	null	null	5643844
2018	null	null	5351343
null	July	Arson	637556
null	June	Unintentional Human Involvement	1650206
null	June	Lightning Strike	746398
null	June	Arson	883686
null	June	Spontaneous Combustion	2042657
null	May	Unintentional Human Involvement	343963
null	July	Lightning Strike	1677810
null	July	Spontaneous Combustion	1910821
null	May	Lightning Strike	1333752
null	May	Spontaneous Combustion	1469949
null	July	Unintentional Human Involvement	1093282
null	May	Arson	2225687
null	null	Lightning Strike	3757960
null	null	Arson	3746929
null	null	Spontaneous Combustion	5423427
null	null	Unintentional Human Involvement	3087451

CUBE with GROUPING() – exercise
Great! As with ROLLUP, you can use the GROUPING() function with CUBE.

Exercise
Show the average damage repair cost for all possible combinations of year, month, and cause. Remove the row containing the total average from the results with the help of the GROUPING() function.

Show the following columns in the query result: year, month, cause, and avg_cost.

SELECT
	year,
    month,
    cause,
    avg(damage_repair_cost)as avg_cost
FROM  wildfire_incident
GROUP BY CUBE(year,month,cause)
HAVING (GROUPING(year) + GROUPING(month) + GROUPING(cause) < 3)

year	month	cause	avg_cost
2017	June	Lightning Strike	746398.000000000000
2016	May	Arson	761208.000000000000
2018	July	Unintentional Human Involvement	561721.000000000000
2018	May	Spontaneous Combustion	473368.000000000000
2017	May	Arson	926046.000000000000
2018	June	Arson	161928.000000000000
2017	July	Spontaneous Combustion	606780.000000000000
2017	May	Lightning Strike	258575.000000000000
2016	June	Spontaneous Combustion	674498.000000000000
2018	June	Unintentional Human Involvement	587734.000000000000
2016	July	Spontaneous Combustion	490614.000000000000
2016	June	Unintentional Human Involvement	885557.000000000000
2017	July	Lightning Strike	723994.000000000000
2018	May	Lightning Strike	722932.000000000000
2016	June	Arson	721758.000000000000
2016	May	Lightning Strike	352245.000000000000
2018	June	Spontaneous Combustion	854244.000000000000
2018	May	Arson	538433.000000000000
2016	July	Lightning Strike	229822.000000000000
2017	June	Spontaneous Combustion	513915.000000000000
2018	July	Spontaneous Combustion	813427.000000000000
2017	May	Unintentional Human Involvement	343963.000000000000
2016	May	Spontaneous Combustion	996581.000000000000
2016	July	Unintentional Human Involvement	531561.000000000000
2018	July	Arson	637556.000000000000
2017	June	Unintentional Human Involvement	176915.000000000000
2017	July	null	684922.666666666667
2016	July	null	417332.333333333333
2017	June	null	479076.000000000000
2016	May	null	703344.666666666667
2018	May	null	578244.333333333333
2018	June	null	534635.333333333333
2017	May	null	509528.000000000000
2018	July	null	670901.333333333333
2016	June	null	760604.333333333333
2017	null	null	557842.222222222222
2016	null	null	627093.777777777778
2018	null	null	594593.666666666667
null	May	Arson	741895.666666666667
null	May	Spontaneous Combustion	734974.500000000000
null	June	Spontaneous Combustion	680885.666666666667
null	July	Arson	637556.000000000000
null	July	Unintentional Human Involvement	546641.000000000000
null	July	Spontaneous Combustion	636940.333333333333
null	May	Unintentional Human Involvement	343963.000000000000
null	June	Unintentional Human Involvement	550068.666666666667
null	July	Lightning Strike	559270.000000000000
null	June	Arson	441843.000000000000
null	June	Lightning Strike	746398.000000000000
null	May	Lightning Strike	444584.000000000000
null	May	null	597039.000000000000
null	July	null	591052.111111111111
null	June	null	591438.555555555556
2017	null	Spontaneous Combustion	560347.500000000000
2017	null	Arson	926046.000000000000
2018	null	Spontaneous Combustion	713679.666666666667
2017	null	Unintentional Human Involvement	260439.000000000000
2016	null	Unintentional Human Involvement	708559.000000000000
2018	null	Arson	445972.333333333333
2018	null	Lightning Strike	722932.000000000000
2017	null	Lightning Strike	613240.250000000000
2016	null	Spontaneous Combustion	720564.333333333333
2018	null	Unintentional Human Involvement	574727.500000000000
2016	null	Lightning Strike	291033.500000000000
2016	null	Arson	741483.000000000000
null	null	Lightning Strike	536851.428571428571
null	null	Arson	624488.166666666667
null	null	Spontaneous Combustion	677928.375000000000
null	null	Unintentional Human Involvement	514575.166666666667

Having clause about filters this row.
null	null	null	593176.555555555556
which would have been the 1st row.

Columns outside CUBE
Good job! Now, if we want to reduce the number of grouping combinations, we can exclude some columns from CUBE:

SELECT
  location,
  gender,
  risk,
  MIN(efficacy) AS min_efficacy
FROM vaccine_administration
GROUP BY 
  CUBE (location, gender), risk;
In the query above, CUBE will create grouping combinations for location and gender, but risk will be added to each grouping combination. As a result, we’ll get the following levels:

GROUP BY CUBE (location, gender), risk =
GROUP BY location, gender, risk +
GROUP BY location, risk +
GROUP BY gender, risk +
GROUP BY risk
Exercise
Run the template query and note that risk is now always used for grouping.

location	gender	risk	min_efficacy
Norway	Male	Medium	2
Argentina	Male	Low	1
Norway	Male	High	1
Argentina	Female	Medium	1
Argentina	Male	High	1
Norway	Female	High	1
Argentina	Male	Medium	1
Norway	Female	Medium	3
Norway	Female	Low	2
Argentina	Female	Low	2
Argentina	Female	High	1
Norway	Male	Low	3
Norway	null	High	1
Norway	null	Medium	2
Argentina	null	Low	1
Argentina	null	Medium	1
Argentina	null	High	1
Norway	null	Low	2
null	null	Medium	1
null	null	High	1
null	null	Low	1
null	Female	Low	2
null	Male	Medium	1
null	Male	Low	1
null	Female	Medium	1
null	Male	High	1
null	Female	High	1

Columns outside CUBE – exercise
Well done! You know the drill; it’s your turn now.

Exercise
Show the average wildfire duration for all grouping combinations based on the year and month columns. Also, group each row by cause.

Show the following columns in the query result: year, month, cause, and avg_duration.

SELECT
	year, 
    month, 
    cause, 
    avg(duration) AS avg_duration
FROM wildfire_incident
GROUP BY CUBE(year, month), cause

year	month	cause	avg_duration
2018	May	Arson	8.0000000000000000
2016	July	Unintentional Human Involvement	23.0000000000000000
2016	July	Spontaneous Combustion	5.0000000000000000
2016	May	Arson	22.0000000000000000
2017	May	Lightning Strike	17.0000000000000000
2017	June	Unintentional Human Involvement	6.0000000000000000
2018	May	Spontaneous Combustion	12.0000000000000000
2017	July	Spontaneous Combustion	16.0000000000000000
2016	May	Spontaneous Combustion	27.0000000000000000
2018	June	Arson	22.0000000000000000
2016	June	Spontaneous Combustion	12.0000000000000000
2016	May	Lightning Strike	7.0000000000000000
2017	July	Lightning Strike	16.5000000000000000
2018	July	Spontaneous Combustion	26.0000000000000000
2018	May	Lightning Strike	8.0000000000000000
2016	June	Unintentional Human Involvement	12.0000000000000000
2018	July	Arson	17.0000000000000000
2017	June	Spontaneous Combustion	14.0000000000000000
2017	May	Arson	17.0000000000000000
2018	June	Unintentional Human Involvement	13.0000000000000000
2018	June	Spontaneous Combustion	4.0000000000000000
2017	June	Lightning Strike	8.0000000000000000
2017	May	Unintentional Human Involvement	21.0000000000000000
2018	July	Unintentional Human Involvement	8.0000000000000000
2016	July	Lightning Strike	9.0000000000000000
2016	June	Arson	12.0000000000000000
2017	null	Spontaneous Combustion	15.0000000000000000
2017	null	Arson	17.0000000000000000
2018	null	Spontaneous Combustion	14.0000000000000000
2017	null	Unintentional Human Involvement	13.5000000000000000
2016	null	Unintentional Human Involvement	17.5000000000000000
2018	null	Arson	15.6666666666666667
2018	null	Lightning Strike	8.0000000000000000
2017	null	Lightning Strike	14.5000000000000000
2016	null	Spontaneous Combustion	14.6666666666666667
2018	null	Unintentional Human Involvement	10.5000000000000000
2016	null	Lightning Strike	8.0000000000000000
2016	null	Arson	17.0000000000000000
null	null	Lightning Strike	11.7142857142857143
null	null	Arson	16.3333333333333333
null	null	Spontaneous Combustion	14.5000000000000000
null	null	Unintentional Human Involvement	13.8333333333333333
null	May	Arson	15.6666666666666667
null	May	Spontaneous Combustion	19.5000000000000000
null	June	Spontaneous Combustion	10.0000000000000000
null	July	Arson	17.0000000000000000
null	July	Unintentional Human Involvement	15.5000000000000000
null	July	Spontaneous Combustion	15.6666666666666667
null	May	Unintentional Human Involvement	21.0000000000000000
null	June	Unintentional Human Involvement	10.3333333333333333
null	July	Lightning Strike	14.0000000000000000
null	June	Arson	17.0000000000000000
null	June	Lightning Strike	8.0000000000000000
null	May	Lightning Strike	10.6666666666666667

CUBE with multiple pairs of parentheses
Good! There is one more interesting modification of CUBE worth mentioning. Take a look:

SELECT
  location,
  gender,
  risk,
  AVG(age) AS avg_age
FROM vaccine_administration
GROUP BY 
  CUBE ((location, gender), risk);
Inside CUBE's parentheses, we put location and gender in another pair of parentheses! This means that location and gender will be treated as a single column – either both or neither of them will be used for grouping:

GROUP BY CUBE((location, gender), risk) =
GROUP BY location, gender, risk +
GROUP BY location, gender +
GROUP BY risk +
GROUP BY ()
Exercise
Run the template query. Note that each row is grouped by either location and gender together or by neither of these columns.

SELECT
  location,
  gender,
  risk,
  AVG(age) AS avg_age
FROM vaccine_administration
GROUP BY CUBE ((location, gender), risk);

location	gender	risk	avg_age
null	null	null	39.4200000000000000
Norway	Male	Medium	29.6666666666666667
Argentina	Male	Low	56.0000000000000000
Norway	Male	High	51.6666666666666667
Argentina	Female	Medium	47.0000000000000000
Argentina	Male	High	37.0000000000000000
Norway	Female	High	37.0000000000000000
Argentina	Male	Medium	36.8571428571428571
Norway	Female	Medium	35.0000000000000000
Norway	Female	Low	34.0000000000000000
Argentina	Female	Low	38.0000000000000000
Argentina	Female	High	44.1428571428571429
Norway	Male	Low	36.6666666666666667
null	null	Medium	36.9411764705882353
null	null	High	42.5294117647058824
null	null	Low	38.7500000000000000
Argentina	Female	null	43.0000000000000000
Argentina	Male	null	40.0833333333333333
Norway	Female	null	35.3333333333333333
Norway	Male	null	38.6666666666666667

CUBE with multiple pairs of parentheses – exercise
All right, it's your turn now.

Exercise
Show the sum of damage_repair_cost for grouping combinations based on the columns year, month, and cause. Treat year and month as a single column.

Show the following columns in the query result: year, month, cause, and sum_damage_repair_cost.
SELECT
	year, 
    month, 
    cause, 
    sum(damage_repair_cost) AS sum_damage_repair_cost
FROM wildfire_incident
GROUP BY CUBE((year, month), cause)

year	month	cause	sum_damage_repair_cost
null	null	null	16015767
2018	May	Arson	538433
2016	July	Unintentional Human Involvement	531561
2016	July	Spontaneous Combustion	490614
2016	May	Arson	761208
2017	May	Lightning Strike	258575
2017	June	Unintentional Human Involvement	176915
2018	May	Spontaneous Combustion	473368
2017	July	Spontaneous Combustion	606780
2016	May	Spontaneous Combustion	996581
2018	June	Arson	161928
2016	June	Spontaneous Combustion	674498
2016	May	Lightning Strike	352245
2017	July	Lightning Strike	1447988
2018	July	Spontaneous Combustion	813427
2018	May	Lightning Strike	722932
2016	June	Unintentional Human Involvement	885557
2018	July	Arson	637556
2017	June	Spontaneous Combustion	513915
2017	May	Arson	926046
2018	June	Unintentional Human Involvement	587734
2018	June	Spontaneous Combustion	854244
2017	June	Lightning Strike	746398
2017	May	Unintentional Human Involvement	343963
2018	July	Unintentional Human Involvement	561721
2016	July	Lightning Strike	229822
2016	June	Arson	721758
null	null	Lightning Strike	3757960
null	null	Arson	3746929
null	null	Spontaneous Combustion	5423427
null	null	Unintentional Human Involvement	3087451
2017	July	null	2054768
2016	July	null	1251997
2017	June	null	1437228
2016	May	null	2110034
2018	May	null	1734733
2018	June	null	1603906
2017	May	null	1528584
2018	July	null	2012704
2016	June	null	2281813

CUBE with COALESCE()
Great! Lastly, the function COALESCE() can be used in the SELECT clause to replace NULL values with the values of your choice:

SELECT
  COALESCE(location, '--') AS location,
  COALESCE(gender, '--') AS gender,
  COALESCE(risk, '--') AS risk,
  AVG(age) AS avg_age
FROM vaccine_administration
GROUP BY 
  CUBE (location, gender, risk);
In the query above, '--' will be shown when location, gender, or risk is NULL.

Exercise
Run the template query and note how NULLs are replaced with '--'.
coalesce	coalesce	coalesce	avg
--	--	--	39.4200000000000000
Argentina	Female	Medium	47.0000000000000000
Norway	Male	Medium	29.6666666666666667
Argentina	Male	Low	56.0000000000000000
Argentina	Female	High	44.1428571428571429
Norway	Female	High	37.0000000000000000
Norway	Female	Low	34.0000000000000000
Argentina	Female	Low	38.0000000000000000
Norway	Male	Low	36.6666666666666667
Argentina	Male	High	37.0000000000000000
Argentina	Male	Medium	36.8571428571428571
Norway	Female	Medium	35.0000000000000000
Norway	Male	High	51.6666666666666667
Argentina	Female	--	43.0000000000000000
Argentina	Male	--	40.0833333333333333
Norway	Female	--	35.3333333333333333
Norway	Male	--	38.6666666666666667
Argentina	--	--	41.6538461538461538
Norway	--	--	37.0000000000000000
--	Female	Low	36.0000000000000000
--	Male	Medium	34.7000000000000000
--	Male	Low	41.5000000000000000
--	Female	Medium	40.1428571428571429
--	Male	High	44.3333333333333333
--	Female	High	41.5454545454545455
--	Female	--	39.4615384615384615
--	Male	--	39.3750000000000000
Norway	--	High	43.2857142857142857
Norway	--	Medium	32.7142857142857143
Argentina	--	Low	44.0000000000000000
Argentina	--	Medium	39.9000000000000000
Argentina	--	High	42.0000000000000000
Norway	--	Low	35.6000000000000000
--	--	Medium	36.9411764705882353
--	--	High	42.5294117647058824
--	--	Low	38.7500000000000000

CUBE with COALESCE() – exercise
Good! Try a short exercise on your own now.

Exercise
Show the average damage repair costs for all grouping combinations based on the year and month columns. Group all rows by cause. Replace any NULL values in month with the word 'ALL'.

Show the following columns in the query result: year, month, cause, and avg_damage_repair_cost.
SELECT
	year, 
    COALESCE(month, 'ALL') AS month, 
    cause, 
    AVG(damage_repair_cost) AS avg_damage_repair_cost
FROM wildfire_incident
GROUP BY CUBE(year, month), cause
year	month	cause	avg_damage_repair_cost
2018	May	Arson	538433.000000000000
2016	July	Unintentional Human Involvement	531561.000000000000
2016	July	Spontaneous Combustion	490614.000000000000
2016	May	Arson	761208.000000000000
2017	May	Lightning Strike	258575.000000000000
2017	June	Unintentional Human Involvement	176915.000000000000
2018	May	Spontaneous Combustion	473368.000000000000
2017	July	Spontaneous Combustion	606780.000000000000
2016	May	Spontaneous Combustion	996581.000000000000
2018	June	Arson	161928.000000000000
2016	June	Spontaneous Combustion	674498.000000000000
2016	May	Lightning Strike	352245.000000000000
2017	July	Lightning Strike	723994.000000000000
2018	July	Spontaneous Combustion	813427.000000000000
2018	May	Lightning Strike	722932.000000000000
2016	June	Unintentional Human Involvement	885557.000000000000
2018	July	Arson	637556.000000000000
2017	June	Spontaneous Combustion	513915.000000000000
2017	May	Arson	926046.000000000000
2018	June	Unintentional Human Involvement	587734.000000000000
2018	June	Spontaneous Combustion	854244.000000000000
2017	June	Lightning Strike	746398.000000000000
2017	May	Unintentional Human Involvement	343963.000000000000
2018	July	Unintentional Human Involvement	561721.000000000000
2016	July	Lightning Strike	229822.000000000000
2016	June	Arson	721758.000000000000
2017	ALL	Spontaneous Combustion	560347.500000000000
2017	ALL	Arson	926046.000000000000
2018	ALL	Spontaneous Combustion	713679.666666666667
2017	ALL	Unintentional Human Involvement	260439.000000000000
2016	ALL	Unintentional Human Involvement	708559.000000000000
2018	ALL	Arson	445972.333333333333
2018	ALL	Lightning Strike	722932.000000000000
2017	ALL	Lightning Strike	613240.250000000000
2016	ALL	Spontaneous Combustion	720564.333333333333
2018	ALL	Unintentional Human Involvement	574727.500000000000
2016	ALL	Lightning Strike	291033.500000000000
2016	ALL	Arson	741483.000000000000
null	ALL	Lightning Strike	536851.428571428571
null	ALL	Arson	624488.166666666667
null	ALL	Spontaneous Combustion	677928.375000000000
null	ALL	Unintentional Human Involvement	514575.166666666667
null	May	Arson	741895.666666666667
null	May	Spontaneous Combustion	734974.500000000000
null	June	Spontaneous Combustion	680885.666666666667
null	July	Arson	637556.000000000000
null	July	Unintentional Human Involvement	546641.000000000000
null	July	Spontaneous Combustion	636940.333333333333
null	May	Unintentional Human Involvement	343963.000000000000
null	June	Unintentional Human Involvement	550068.666666666667
null	July	Lightning Strike	559270.000000000000
null	June	Arson	441843.000000000000
null	June	Lightning Strike	746398.000000000000
null	May	Lightning Strike	444584.000000000000

Summary
Great! It’s time to wrap things up.

GROUP BY CUBE () creates all possible grouping combinations with the columns inside its parentheses.
The order of columns within the parentheses of CUBE doesn't matter.
You can use the GROUPING() function to show if the column is included in the grouping.
You can leave some columns outside CUBE. Such columns will always be used for grouping.
You can use additional pairs of parentheses inside CUBE to indicate that certain columns should be treated as a single column by the CUBE clause.
You can use COALESCE() to replace NULL values with something more meaningful.

Exercise 1
For this quiz, we’ll be using a small table named customer_order. The following columns are available: id, country, customer, customization, and total_price. You can use the button on the right side to analyze this table’s contents before we start.

Exercise
Find the average total price in all grouping combinations made from the following columns: country, customer, and customization.

Show the following columns in the query result: country, customer, customization, and avg_price.


SELECT 
	country, 
    customer,
    customization,
	AVG(total_price) AS avg_price
FROM  customer_order
GROUP BY CUBE(country,customer,customization)

country	customer	customization	avg_price
null	null	null	343563.250000000000
Mexico	XYZ Limited	High	410600.000000000000
Mexico	XYZ Limited	Low	323264.500000000000
Mexico	ABC Company	High	351629.500000000000
Mexico	ABC Company	Low	292441.500000000000
Malaysia	A & A Holding	High	283083.500000000000
Malaysia	A & A Holding	Low	126091.500000000000
Malaysia	B2B Corp.	High	399750.000000000000
Malaysia	B2B Corp.	Low	561645.500000000000
Malaysia	A & A Holding	null	204587.500000000000
Mexico	ABC Company	null	322035.500000000000
Malaysia	B2B Corp.	null	480697.750000000000
Mexico	XYZ Limited	null	366932.250000000000
Malaysia	null	null	342642.625000000000
Mexico	null	null	344483.875000000000
null	A & A Holding	Low	126091.500000000000
null	B2B Corp.	High	399750.000000000000
null	A & A Holding	High	283083.500000000000
null	B2B Corp.	Low	561645.500000000000
null	XYZ Limited	Low	323264.500000000000
null	ABC Company	Low	292441.500000000000
null	XYZ Limited	High	410600.000000000000
null	ABC Company	High	351629.500000000000
null	XYZ Limited	null	366932.250000000000
null	B2B Corp.	null	480697.750000000000
null	ABC Company	null	322035.500000000000
null	A & A Holding	null	204587.500000000000
Malaysia	null	High	341416.750000000000
Mexico	null	Low	307853.000000000000
Malaysia	null	Low	343868.500000000000
Mexico	null	High	381114.750000000000
null	null	High	361265.750000000000
null	null	Low	325860.750000000000

Exercise 2
Great! One more question.

Exercise
Find the maximum total price in all grouping combinations based on the country and customer columns. Group all rows by the customization column. Instead of NULLs in the country and customer columns, show a double dash (--) in the country and customer columns.

Show the following columns in the query result: country, customer, customization, and max_total_price.

SELECT 
	COALESCE(country,'--')AS country, 
    COALESCE(customer,'--')AS customer,
    customization,
    MAX(total_price) AS max_total_price
FROM  customer_order
GROUP BY CUBE(country, customer), 
  customization;

  country	customer	customization	max_total_price
Mexico	ABC Company	High	547954
Mexico	ABC Company	Low	369167
Malaysia	A & A Holding	Low	145864
Mexico	XYZ Limited	High	494115
Malaysia	B2B Corp.	Low	599271
Mexico	XYZ Limited	Low	383119
Malaysia	A & A Holding	High	338818
Malaysia	B2B Corp.	High	449642
Malaysia	--	High	449642
Mexico	--	Low	383119
Malaysia	--	Low	599271
Mexico	--	High	547954
--	--	High	547954
--	--	Low	599271
--	A & A Holding	Low	145864
--	B2B Corp.	High	449642
--	A & A Holding	High	338818
--	B2B Corp.	Low	599271
--	XYZ Limited	Low	383119
--	ABC Company	Low	369167
--	XYZ Limited	High	494115
--	ABC Company	High	547954

-------------------------------------------------------------------------------------------
3. GROUPING SETS
Let's take a close look at GROUPING SETS, the last GROUP BY extension in our course.

Introduction
Welcome back!

So far, you've learned how to use CUBE and ROLLUP for advanced reporting purposes. This time, we'll present GROUPING SETS – the third and last GROUP BY extension in this course.

Get to know the warranty_repair table
Let's get to know the tables we'll be working with in this part.

We'll use a table named warranty_repair in our examples. It has the following columns:

id – the ID of the repair.
customer_id – the ID of the customer who ordered a given repair.
repair_center – denotes which warranty center repaired the device (USA, Germany, or Japan).
date_received – the date when the repair center received the device for repair.
repair_duration – the number of days it took to repair the device.
repair_cost – the USD cost of spare parts used to repair the device (0 in the case of software problems).

id	customer_id	repair_center	date_received	repair_duration	repair_cost
1	1032245	USA	2018-08-01	2	0
2	1032245	USA	2018-08-01	7	189
3	1032245	USA	2018-08-01	3	97
4	1032245	USA	2018-08-02	14	0
5	1032245	Germany	2018-08-03	1	14
6	1032245	Germany	2018-08-02	13	54
7	1032245	Germany	2018-08-03	6	76
8	1032245	Japan	2018-08-01	4	0
9	1032245	Japan	2018-08-03	6	458
10	1465248	USA	2018-08-02	1	234
11	1465248	USA	2018-08-03	8	243
12	1465248	USA	2018-08-01	10	0
13	1465248	Germany	2018-08-02	2	16
14	1465248	Germany	2018-08-03	14	32
15	1465248	Germany	2018-08-01	8	34
16	1465248	Germany	2018-08-02	7	0
17	1465248	Japan	2018-08-03	3	153
18	1465248	Japan	2018-08-03	3	0
19	1465248	Japan	2018-08-01	8	0

Get to know the loan table
Well done! Now let's take a look at the table you'll be working with in the exercises. It's named loan and it contains information on loans made by an imaginary bank to imaginary clients.

id – the ID of the loan.
client_id – the ID of the client.
sales_person – the first and last name of the sales person.
country – the country where the loan was made.
year – the year in which the loan was taken out.
quarter – the yearly quarter when the loan was taken out.
principal – the amount borrowed.
interest – the interest rate on the loan.

id	client_id	sales_person	country	year	quarter	principal	interest
1	1	Margaret Lawson	France	2017	1	238000	2
2	2	Margaret Lawson	France	2017	2	258000	27
3	3	Margaret Lawson	France	2017	3	121000	6
4	1	Nathaniel Norman	France	2018	4	448000	22
5	4	Nathaniel Norman	France	2018	1	448000	16
6	5	Nathaniel Norman	France	2018	2	324000	18
7	6	Adam Bishop	Spain	2017	3	64000	1
8	7	Adam Bishop	Spain	2017	4	334000	8
9	7	Adam Bishop	Spain	2017	1	388000	11
10	8	Preston Jones	Spain	2017	2	203000	6
11	8	Preston Jones	Spain	2018	3	358000	7
12	9	Preston Jones	Spain	2018	4	56000	24
13	6	Anna Ball	Spain	2018	1	239000	24
14	10	Anna Ball	Spain	2018	2	383000	26
15	11	Dale Parker	Italy	2017	3	121000	14
16	12	Dale Parker	Italy	2017	4	476000	13
17	11	Eula Hoffman	Italy	2017	1	45000	28
18	13	Eula Hoffman	Italy	2018	2	285000	22
19	14	Carrie Walsh	Italy	2018	3	76000	4
20	15	Carrie Walsh	Italy	2018	4	381000	29

Multiple grouping with UNION ALL
All right, let's get started with GROUPING SETS.

Imagine that you need to create a report on the average repair duration for two grouping levels:

per customer_id and repair_center.
per date_received date.
You don't want to create any additional grouping levels because you need the report to stay clear and simple.

There is no way you could use either ROLLUP or CUBE to create those grouping levels. One thing you could do is write two separate queries and join them with UNION ALL:

SELECT
  NULL AS date_received, 
  customer_id, 
  repair_center, 
  AVG(repair_duration) AS avg_repair_duration
FROM warranty_repair
GROUP BY customer_id, repair_center
UNION ALL
SELECT
  date_received, 
  NULL, 
  NULL, 
  AVG(repair_duration) AS avg_repair_duration
FROM warranty_repair
GROUP BY date_received
Note that UNION ALL requires both queries to have the same number of columns. This is why we needed to add some NULLs as columns in the SELECT clause.

Exercise
Run the template query to see how it works.

date_received	customer_id	repair_center	avg_repair_duration
null	1465248	Japan	4.6666666666666667
null	1032245	Germany	6.6666666666666667
null	1465248	USA	6.3333333333333333
null	1032245	USA	6.5000000000000000
null	1032245	Japan	5.0000000000000000
null	1465248	Germany	7.7500000000000000
2018-08-02	null	null	7.4000000000000000
2018-08-03	null	null	5.8571428571428571
2018-08-01	null	null	6.0000000000000000

Multiple grouping with UNION ALL – practice
Great! Now it's your turn to write a similar query.

Try this exercise!

Exercise
Find the average principal amounts for the following two grouping combinations:

year and quarter
country
Show the following columns in the query result: year, quarter, country, and avg_principal.




SELECT
  year,
  quarter,
  NULL AS country,
  AVG(principal) AS avg_principal
FROM loan
GROUP BY year, quarter
UNION ALL
SELECT
  NULL AS year,
  NULL AS quarter,
  country,
  AVG(principal)
FROM loan
GROUP BY country;


year	quarter	country	avg_principal
2017	4	null	405000.000000000000
2017	2	null	230500.000000000000
2017	1	null	223666.666666666667
2017	3	null	102000.000000000000
2018	2	null	330666.666666666667
2018	3	null	217000.000000000000
2018	1	null	343500.000000000000
2018	4	null	295000.000000000000
null	null	France	306166.666666666667
null	null	Spain	253125.000000000000
null	null	Italy	230666.666666666667

Multiple groupings with GROUPING SETS
Good job! As you saw, using UNION ALL is one way of dealing with such reports, but it seems to cause all kinds of problems:

The statement becomes huge as more queries are added with UNION ALL
The table has to be accessed multiple times, which affects performance
Adding NULL values in the SELECT clause is awkward and error prone
That's where GROUPING SETS come in handy. They allow you to perform multiple groupings within a single query; each grouping is explicitly stated, as we see below:

SELECT
  date_received, 
  customer_id,   
  repair_center, 
  AVG(repair_duration) AS avg_repair_duration
FROM warranty_repair
GROUP BY GROUPING SETS
(
  (customer_id, repair_center),
  (date_received)
)
As you can see, GROUP BY GROUPING SETS is followed by a pair of parentheses. Inside, we put all the grouping combinations we wish to get, each in a separate pair of parentheses and separated by a comma.

Exercise
Run the template query. Note how two different grouping levels are created.

SELECT
  date_received, 
  customer_id, 
  repair_center, 
  AVG(repair_duration) AS avg_repair_duration
FROM warranty_repair
GROUP BY GROUPING SETS
(
  (customer_id, repair_center),
  (date_received)
)

date_received	customer_id	repair_center	avg_repair_duration
2018-08-02	null	null	7.4000000000000000
2018-08-03	null	null	5.8571428571428571
2018-08-01	null	null	6.0000000000000000
null	1465248	Japan	4.6666666666666667
null	1032245	Germany	6.6666666666666667
null	1465248	USA	6.3333333333333333
null	1032245	USA	6.5000000000000000
null	1032245	Japan	5.0000000000000000
null	1465248	Germany	7.7500000000000000

Multiple groupings with GROUPING SETS – practice
Okay, it's time to practice GROUPING SETS.

Exercise
Rewrite the template query so it uses GROUPING SETS instead of UNION ALL.
SELECT
  year,
  quarter,
  country,
  AVG(principal) AS avg_principal
FROM loan
GROUP BY GROUPING SETS(
  (year, quarter),
  (country)
)

year	quarter	country	avg_principal
null	null	France	306166.666666666667
null	null	Spain	253125.000000000000
null	null	Italy	230666.666666666667
2017	4	null	405000.000000000000
2017	2	null	230500.000000000000
2017	1	null	223666.666666666667
2017	3	null	102000.000000000000
2018	2	null	330666.666666666667
2018	3	null	217000.000000000000
2018	1	null	343500.000000000000
2018	4	null	295000.000000000000

Using an empty grouping set
Well done! Let's say we also need to add the general average repair duration to our report. To that end, we can use an empty pair of parentheses:

SELECT
  date_received, 
  customer_id, 
  repair_center, 
  AVG(repair_duration) AS avg_repair_duration
FROM warranty_repair
GROUP BY GROUPING SETS
(
  (customer_id, repair_center),
  (date_received),
  ()
)
Exercise
Run the template query. As you can see, a row with a general average was added.

date_received	customer_id	repair_center	avg_repair_duration
null	null	null	6.3157894736842105
2018-08-02	null	null	7.4000000000000000
2018-08-03	null	null	5.8571428571428571
2018-08-01	null	null	6.0000000000000000
null	1465248	Japan	4.6666666666666667
null	1032245	Germany	6.6666666666666667
null	1465248	USA	6.3333333333333333
null	1032245	USA	6.5000000000000000
null	1032245	Japan	5.0000000000000000
null	1465248	Germany	7.7500000000000000

Using an empty grouping set – practice
Nicely done! Now it's your turn.

Exercise
Find the average principal (as avg_principal) and average interest (as avg_interest) for the following grouping combinations:

year and quarter
country
None (overall averages)
SELECT
	year,
    quarter,
    country,
    AVG(principal) AS avg_principal,
    AVG(interest) AS avg_interest
FROM  loan
GROUP BY GROUPING SETS
	(    
    (year, quarter),
    (country),
      ()
      )
      year	quarter	country	avg_principal	avg_interest
null	null	null	262300.000000000000	15.4000000000000000
null	null	France	306166.666666666667	15.1666666666666667
null	null	Spain	253125.000000000000	13.3750000000000000
null	null	Italy	230666.666666666667	18.3333333333333333
2017	4	null	405000.000000000000	10.5000000000000000
2017	2	null	230500.000000000000	16.5000000000000000
2017	1	null	223666.666666666667	13.6666666666666667
2017	3	null	102000.000000000000	7.0000000000000000
2018	2	null	330666.666666666667	22.0000000000000000
2018	3	null	217000.000000000000	5.5000000000000000
2018	1	null	343500.000000000000	20.0000000000000000
2018	4	null	295000.000000000000	25.0000000000000000

GROUPING SETS with GROUPING()
Great! Of course, GROUPING SETS works with the GROUPING() function too:

SELECT
  date_received,
  customer_id,
  repair_center,
  AVG(repair_duration) AS avg_repair_duration,
  GROUPING(date_received) AS D,
  GROUPING(customer_id) AS C,
  GROUPING(repair_center) AS R
FROM warranty_repair
GROUP BY GROUPING SETS
(
  (customer_id, repair_center),
  (date_received)
)
Exercise
Find the average interest amounts for the following grouping levels:

year and quarter
country
Show the following columns in the query result: year, quarter, country, avg_interest, Y, Q, and C. The Y, Q, and C columns show if the columns year, quarter, and country were used in the grouping.

SELECT
	year,
    quarter,
    country,
    AVG(interest) AS avg_interest,
    GROUPING(year) AS Y,
  	GROUPING(quarter) AS Q,
  	GROUPING(country) AS C
FROM  loan
GROUP BY GROUPING SETS
(    
    (year, quarter),
    (country)
)

year	quarter	country	avg_interest	y	q	c
null	null	France	15.1666666666666667	1	1	0
null	null	Spain	13.3750000000000000	1	1	0
null	null	Italy	18.3333333333333333	1	1	0
2017	4	null	10.5000000000000000	0	0	1
2017	2	null	16.5000000000000000	0	0	1
2017	1	null	13.6666666666666667	0	0	1
2017	3	null	7.0000000000000000	0	0	1
2018	2	null	22.0000000000000000	0	0	1
2018	3	null	5.5000000000000000	0	0	1
2018	1	null	20.0000000000000000	0	0	1
2018	4	null	25.0000000000000000	0	0	1

GROUPING SETS with COALESCE()
Nice job!

What can we do with those NULL values? Naturally, we can use COALESCE():

SELECT
  date_received,
  customer_id,
  COALESCE(repair_center, 'ALL'),
  AVG(repair_duration) AS avg_repair_duration
FROM warranty_repair
GROUP BY GROUPING SETS
(
  (customer_id, repair_center),
  (date_received)
)
In the query above, any NULL values in the repair_center column will be replaced with the word ALL.

Exercise
Show the sum of all principal amounts for two grouping sets:

sales_person
country
Show the following columns in the query result: sales_person, country, and Sumprincipal.

Instead of NULL values in the columns sales_person and country, show a double dash (--).

 SELECT
	COALESCE(sales_person,'--') as sales_person,
    COALESCE(country, '--') as country,
    Sum(principal) as Sumprincipal
FROM  loan
GROUP BY GROUPING SETS
(    
   sales_person,country
)
sales_person	country	sumprincipal
Eula Hoffman	--	330000
Adam Bishop	--	786000
Preston Jones	--	617000
Nathaniel Norman	--	1220000
Margaret Lawson	--	617000
Carrie Walsh	--	457000
Dale Parker	--	597000
Anna Ball	--	622000
--	France	1837000
--	Spain	2025000
--	Italy	1384000
GROUPING SETS with ROLLUP and CUBE
Good job! One more interesting thing we can do with GROUPING SETS is combine them with ROLLUP or CUBE:

SELECT
  date_received,
  customer_id,
  repair_center,
  AVG(repair_duration) AS avg_repair_duration
FROM warranty_repair
GROUP BY GROUPING SETS
(
  ROLLUP (customer_id, repair_center),
  (date_received)
)
The query above will merge ROLLUP grouping levels with a grouping level based on date_received. As a result, we'll get the following grouping combinations:

LEAD

Exercise
Run the template query and note how many grouping levels are created.

date_received	customer_id	repair_center	avg_repair_duration
null	null	null	6.3157894736842105
null	1465248	Japan	4.6666666666666667
null	1032245	Germany	6.6666666666666667
null	1465248	USA	6.3333333333333333
null	1032245	USA	6.5000000000000000
null	1032245	Japan	5.0000000000000000
null	1465248	Germany	7.7500000000000000
null	1032245	null	6.2222222222222222
null	1465248	null	6.4000000000000000
2018-08-02	null	null	7.4000000000000000
2018-08-03	null	null	5.8571428571428571
2018-08-01	null	null	6.0000000000000000

GROUPING SETS with ROLLUP and CUBE – practice
How about trying this exercise?

Exercise
Show the average interest amounts for the following grouping sets:

ROLLUP by year and quarter
country
Show the following columns in the query result: year, quarter, country, and avg_interest.
 SELECT
	year,
    quarter,
    country,
    AVG(interest) AS avg_interest
FROM  loan
GROUP BY GROUPING SETS
(    
   ROLLUP(year, quarter),
    (country)
)

year	quarter	country	avg_interest
null	null	null	15.4000000000000000
2017	4	null	10.5000000000000000
2017	2	null	16.5000000000000000
2017	1	null	13.6666666666666667
2017	3	null	7.0000000000000000
2018	2	null	22.0000000000000000
2018	3	null	5.5000000000000000
2018	1	null	20.0000000000000000
2018	4	null	25.0000000000000000
2017	null	null	11.6000000000000000
2018	null	null	19.2000000000000000
null	null	France	15.1666666666666667
null	null	Spain	13.3750000000000000
null	null	Italy	18.3333333333333333

Summary
Good job! It's time to wrap things up.

GROUP BY GROUPING SETS( ) works by creating the grouping levels provided explicitly within its parentheses.
You can use a pair of empty parentheses ( ) to introduce a general average, sum, etc.
The GROUPING SETS extension can be combined with COALESCE().
You can use both ROLLUP and CUBE to create even more sophisticated grouping combinations.

SELECT
	COALESCE(site,'ALL') AS site, 
    year, 
    COALESCE(weather,'ALL') AS weather,
    avg(amount) AS avg_amount
FROM fruit_harvest
GROUP BY GROUPING SETS
(
    (site,year),
	weather
)  
site	year	weather	avg_amount
ALL	null	Poor	111.1111111111111111
ALL	null	Fair	181.5555555555555556
B	2016	ALL	28.5000000000000000
C	2017	ALL	284.5000000000000000
A	2017	ALL	113.5000000000000000
A	2016	ALL	106.0000000000000000
B	2017	ALL	33.0000000000000000
A	2018	ALL	123.0000000000000000
B	2018	ALL	65.5000000000000000
C	2018	ALL	281.5000000000000000
C	2016	ALL	281.5000000000000000
Exercise 2
Good job! One more question.

Exercise
Show the total amount of the harvest for the following grouping sets:

ROLLUP with site and year
weather
Show the following columns in the result: site, year, weather, and total_amount.

SELECT
	site, 
    year, 
    weather, 
    SUM(amount) as total_amount
FROM fruit_harvest
GROUP BY GROUPING SETS
(
  ROLLUP(SITE,YEAR),
  	weather
)  

site	year	weather	total_amount
null	null	null	2634
B	2016	null	57
C	2017	null	569
A	2017	null	227
A	2016	null	212
B	2017	null	66
A	2018	null	246
B	2018	null	131
C	2018	null	563
C	2016	null	563
B	null	null	254
C	null	null	1695
A	null	null	685
null	null	Poor	1000
null	null	Fair	1634
--------------------------------------------------------------------------------------------
4.
Final Quiz
Verify how much you've learned with us in this course.
Introduction
Welcome! In this part, you'll get a chance to check how much you've learned throughout our GROUP BY Extensions in PostgreSQL course.

You will be presented with five questions covering several of the topics you learned in this course.

When in doubt, you can always take a look at the hints we've prepared. If they aren't enough, feel free to revisit the previous parts of the course and do a thorough preview.

Good luck, and have fun!

Get to know the company_car_trip table
Before we start, let's get to know the table we'll be working with. It's named company_car_trip and it describes trips made by the company cars of an imaginary business. The table contains the following columns:

id – the ID of the trip.
year – the year of the trip.
month – the month of the trip.
day – the day of the trip.
country – the country in which the trip was taken.
car_size – the size of the car driven (either Small or Big).
trip_type – the type of trip (either BusinessTrip or ProductTransport).
distance – the distance driven, in km.
fuel_consumed – the amount of fuel consumed during the trip, in liters.
Exercise
Select all information from the company_car_trip table.

id	year	month	day	country	car_size	trip_type	distance	fuel_consumed
1	2017	1	15	Germany	Small	BusinessTrip	56	5.02
2	2017	1	15	Greece	Small	BusinessTrip	121	11.88
3	2017	1	15	Australia	Small	ProductTransport	209	19.47
4	2017	1	15	Brazil	Small	ProductTransport	407	26.92
5	2017	1	15	Germany	Small	BusinessTrip	328	30.75
6	2017	2	8	Greece	Small	BusinessTrip	489	40.56
7	2017	2	8	Australia	Small	ProductTransport	94	8.45
8	2017	2	8	Brazil	Small	ProductTransport	217	19.95
9	2017	2	8	Germany	Small	BusinessTrip	265	14.13
10	2017	2	8	Greece	Small	BusinessTrip	226	17.10
11	2017	2	8	Australia	Small	ProductTransport	350	30.06
12	2017	3	24	Brazil	Small	ProductTransport	421	36.63
13	2017	3	24	Germany	Small	BusinessTrip	448	27.05
14	2017	3	24	Greece	Small	BusinessTrip	143	10.76
15	2017	3	24	Australia	Small	ProductTransport	118	8.27
16	2017	3	24	Brazil	Big	ProductTransport	392	54.88
17	2018	1	4	Germany	Big	BusinessTrip	406	38.54
18	2018	1	4	Greece	Big	BusinessTrip	235	30.64
19	2018	1	4	Australia	Big	ProductTransport	218	18.49
20	2018	1	4	Brazil	Big	ProductTransport	262	33.27
21	2018	1	4	Germany	Big	BusinessTrip	227	19.85
22	2018	2	3	Greece	Big	BusinessTrip	334	45.96
23	2018	2	3	Australia	Big	ProductTransport	406	35.09
24	2018	2	3	Brazil	Big	ProductTransport	207	16.86
25	2018	2	3	Germany	Big	BusinessTrip	113	16.29
26	2018	2	3	Greece	Big	BusinessTrip	123	17.55
27	2018	3	16	Australia	Big	ProductTransport	207	23.32
28	2018	3	16	Brazil	Big	ProductTransport	199	21.04
29	2018	3	16	Germany	Big	BusinessTrip	58	4.72
30	2018	3	16	Greece	Big	BusinessTrip	450	46.71
31	2018	3	16	Australia	Big	ProductTransport	421	53.03

Exercise
Show the average distance driven for the following grouping combinations:

year, month, and day
year and month
year
None (general average)
In the query result, show the following columns: year, month, day, and avg_distance.

SELECT
	year, 
    month, 
    day, 
    avg(distance) AS avg_distance
FROM company_car_trip
GROUP BY ROLLUP(year, month, day)

year	month	day	avg_distance
null	null	null	262.9032258064516129
2017	1	15	224.2000000000000000
2018	1	4	269.6000000000000000
2017	2	8	273.5000000000000000
2017	3	24	304.4000000000000000
2018	2	3	236.6000000000000000
2018	3	16	267.0000000000000000
2017	2	null	273.5000000000000000
2017	1	null	224.2000000000000000
2017	3	null	304.4000000000000000
2018	2	null	236.6000000000000000
2018	3	null	267.0000000000000000
2018	1	null	269.6000000000000000
2017	null	null	267.7500000000000000
2018	null	null	257.7333333333333333

Exercise
Find the maximum amount of fuel consumed for all grouping combinations involving the country, car_size, and trip_type columns. Show the following columns in the query result: country, car_size, trip_type, and max_fuel_consumed. Replace all NULL values in the first three columns with a double dash --.

SELECT 
	COALESCE(country,'--') AS country, 
    COALESCE(car_size,'--') AS car_size,
    COALESCE(trip_type, '--') AS trip_type,
    MAX(fuel_consumed) AS max_fuel_consumed
FROM company_car_trip
GROUP BY CUBE(country, 
    car_size,
    trip_type)
country	car_size	trip_type	max_fuel_consumed
--	--	--	54.88
Germany	Small	BusinessTrip	30.75
Germany	Big	BusinessTrip	38.54
Australia	Big	ProductTransport	53.03
Greece	Big	BusinessTrip	46.71
Brazil	Small	ProductTransport	36.63
Brazil	Big	ProductTransport	54.88
Greece	Small	BusinessTrip	40.56
Australia	Small	ProductTransport	30.06
Australia	Big	--	53.03
Brazil	Big	--	54.88
Australia	Small	--	30.06
Greece	Big	--	46.71
Greece	Small	--	40.56
Germany	Big	--	38.54
Brazil	Small	--	36.63
Germany	Small	--	30.75
Australia	--	--	53.03
Germany	--	--	38.54
Greece	--	--	46.71
Brazil	--	--	54.88
--	Big	ProductTransport	54.88
--	Big	BusinessTrip	46.71
--	Small	BusinessTrip	40.56
--	Small	ProductTransport	36.63
--	Big	--	54.88
--	Small	--	40.56
Germany	--	BusinessTrip	38.54
Brazil	--	ProductTransport	54.88
Greece	--	BusinessTrip	46.71
Australia	--	ProductTransport	53.03
--	--	ProductTransport	54.88
--	--	BusinessTrip	46.71

Question 3
Well done! How about Question 3?

Exercise
Find the average distance driven for the following grouping combinations:

year, month, and day
country and car_size
Show the following columns in the query result: year, month, day, country, car_size, and avg_distance.

SELECT 
	year, month, day, country, car_size,
    AVG(distance) AS avg_distance
FROM company_car_trip
GROUP BY GROUPING SETS
(
  	(year, month, day),
  	(country, car_size)
)  
year	month	day	country	car_size	avg_distance
null	null	null	Australia	Big	313.0000000000000000
null	null	null	Brazil	Big	265.0000000000000000
null	null	null	Australia	Small	192.7500000000000000
null	null	null	Greece	Big	285.5000000000000000
null	null	null	Greece	Small	244.7500000000000000
null	null	null	Germany	Big	201.0000000000000000
null	null	null	Brazil	Small	348.3333333333333333
null	null	null	Germany	Small	274.2500000000000000
2017	1	15	null	null	224.2000000000000000
2018	1	4	null	null	269.6000000000000000
2017	2	8	null	null	273.5000000000000000
2017	3	24	null	null	304.4000000000000000
2018	2	3	null	null	236.6000000000000000
2018	3	16	null	null	267.0000000000000000

Question 4
Good job. Two more questions to go!

Exercise
Find the minimum fuel consumption per 100 km for all grouping combinations involving the year, country, and car_size columns.

Show the following columns in the query result: year, country, car_size, y, c, cs showing if the columns year, country, car_size, respectively, is included in the grouping, and min_fuel_consumption.

To calculate fuel consumption per 100 km, divide fuel_consumed by distance, and multiply the result by 100.

SELECT 
	year, country, car_size,
    GROUPING(year) AS Y,
    GROUPING(country) AS C,
    GROUPING(car_size) AS CS,
    MIN(fuel_consumed/distance*100) AS min_fuel_consumption
FROM company_car_trip
GROUP BY cube(year,country, car_size)
  
year	country	car_size	y	c	cs	min_fuel_consumption
null	null	null	1	1	1	5.33207547169811320800
2017	Germany	Small	0	0	0	5.33207547169811320800
2018	Germany	Big	0	0	0	8.13793103448275862100
2018	Brazil	Big	0	0	0	8.14492753623188405800
2017	Greece	Small	0	0	0	7.52447552447552447600
2017	Brazil	Small	0	0	0	6.61425061425061425100
2018	Greece	Big	0	0	0	10.38000000000000000000
2017	Australia	Small	0	0	0	7.00847457627118644100
2018	Australia	Big	0	0	0	8.48165137614678899100
2017	Brazil	Big	0	0	0	14.00000000000000000000
2017	Greece	null	0	0	1	7.52447552447552447600
2018	Greece	null	0	0	1	10.38000000000000000000
2018	Australia	null	0	0	1	8.48165137614678899100
2018	Germany	null	0	0	1	8.13793103448275862100
2017	Germany	null	0	0	1	5.33207547169811320800
2017	Brazil	null	0	0	1	6.61425061425061425100
2017	Australia	null	0	0	1	7.00847457627118644100
2018	Brazil	null	0	0	1	8.14492753623188405800
2017	null	null	0	1	1	5.33207547169811320800
2018	null	null	0	1	1	8.13793103448275862100
null	Australia	Big	1	0	0	8.48165137614678899100
null	Brazil	Big	1	0	0	8.14492753623188405800
null	Australia	Small	1	0	0	7.00847457627118644100
null	Greece	Big	1	0	0	10.38000000000000000000
null	Greece	Small	1	0	0	7.52447552447552447600
null	Germany	Big	1	0	0	8.13793103448275862100
null	Brazil	Small	1	0	0	6.61425061425061425100
null	Germany	Small	1	0	0	5.33207547169811320800
null	Australia	null	1	0	1	7.00847457627118644100
null	Germany	null	1	0	1	5.33207547169811320800
null	Greece	null	1	0	1	7.52447552447552447600
null	Brazil	null	1	0	1	6.61425061425061425100
2018	null	Big	0	1	0	8.13793103448275862100
2017	null	Small	0	1	0	5.33207547169811320800
2017	null	Big	0	1	0	14.00000000000000000000
null	null	Big	1	1	0	8.13793103448275862100
null	null	Small	1	1	0	5.33207547169811320800

Question 5
Well done! We have one more question for you.

Exercise
Find the average fuel cost rounded to two decimal places for the following grouping combinations:

year, month, and day
year and month
year
car_size and trip_type
country
Show also the general average fuel cost.

Assume a fuel price of $0.98 per liter. Show the following columns in the query result: year, month, day, car_size, trip_type, country, and avg_fuel_cost.

SELECT 
	year, month, day, car_size, trip_type, country, 
    ROUND(AVG(fuel_consumed*.98),2) AS avg_fuel_cost
FROM company_car_trip
GROUP BY GROUPING SETS
(
  	ROLLUP(year, month, day),
  	(car_size, trip_type),
  	country
)  

year	month	day	car_size	trip_type	country	avg_fuel_cost
null	null	null	null	null	null	24.76
2017	1	15	null	null	null	18.43
2018	1	4	null	null	null	27.59
2017	2	8	null	null	null	21.27
2017	3	24	null	null	null	26.97
2018	2	3	null	null	null	25.82
2018	3	16	null	null	null	29.17
2017	2	null	null	null	null	21.27
2017	1	null	null	null	null	18.43
2017	3	null	null	null	null	26.97
2018	2	null	null	null	null	25.82
2018	3	null	null	null	null	29.17
2018	1	null	null	null	null	27.59
2017	null	null	null	null	null	22.17
2018	null	null	null	null	null	27.53
null	null	null	null	null	Australia	24.03
null	null	null	null	null	Germany	19.15
null	null	null	null	null	Greece	27.09
null	null	null	null	null	Brazil	29.34
null	null	null	Big	ProductTransport	null	31.36
null	null	null	Big	BusinessTrip	null	26.98
null	null	null	Small	BusinessTrip	null	19.26
null	null	null	Small	ProductTransport	null	20.97

Congratulations
Good job! You managed to solve all of the questions we prepared. Congratulations!

At this point, you should know enough about GROUP BY extensions to use them on your own. Thanks for studying with us! If you want to learn more, check out the other courses we offer. Have you already tried the Recursive Queries in PostgreSQL course? It teaches you how to write recursive CTEs. You'll also learn how to process trees and graphs in T-SQL and how to effectively organize long SQL queries.
    
