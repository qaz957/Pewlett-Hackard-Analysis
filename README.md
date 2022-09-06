# Pewlett Hackard Analysis

## Overview
Perform an anlysis of employee data from a large coporation to generate statistics about upcoming retirements to predict future hiring and training needs.

## Resources
Tools: PostgrSQL, pgAdmin, SQL Queries

## Analysis
- We found through analysis of the raw data from Pewlett-Hackard using SQL queries to refine the data, they will have a total of 90,398 employees eligibile for retirement.
- We can also see that the majority of retiring positions are from the engineering department.

![retiring_titles](https://user-images.githubusercontent.com/108296899/188714371-d6a564fc-32c4-45fe-a18c-5546c23381d5.png)


SQL Queries:
  
    SELECT e.emp_no,
      e.first_name,
      e.last_name,
      t.title, t.from_date, t.to_date
    INTO retiree_titles
    FROM employees as e
    JOIN title as t
    ON e.emp_no = t.emp_no
    WHERE e.birth_date BETWEEN '1952-01-01' AND '1955-12-31'
    ORDER BY e.emp_no; 

    SELECT DISTINCT ON (emp_no) emp_no,
      first_name,
      last_name,
      title,
      to_date
    INTO unique_titles 
    FROM retiree_titles 
    ORDER BY emp_no, to_date DESC;

    SELECT count(title) "count", title
    INTO retiring_titles
    FROM unique_titles
    GROUP BY title
    ORDER BY count DESC;
  
- To fill the knowledge gap being left behind, we looked into the possibilty of a mentorship program that would put near retirement employees in mentor roles of newer employees.
- By department, these are the number of eligible mentors currently at PH.

![mentorship_eligibilty](https://user-images.githubusercontent.com/108296899/188715659-803b53f5-e76a-4011-81a7-5d70b0a1a57a.png)

SQL Queries:

    SELECT DISTINCT ON (emp_no) e.emp_no,
        e.first_name,
         e.last_name,
         e.birth_date,
         de.from_date,
         de.to_date,
         t.title
    INTO mentorship_eligibility
    FROM employees as e
    JOIN dept_emp as de
    ON (e.emp_no = de.emp_no)
    JOIN title as t
    ON (e.emp_no = t.emp_no)
    WHERE (de.to_date = '9999-01-01') AND (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
    ORDER BY e.emp_no;
    
## Summary

Our analysis indicates that Pewlett-Hackard has over 90,000 eployees eligiible for retirement and should plan for them to do so within the next 1 to 5 years. Currently, there are 1,549 employees availabe to be mentors for the mentorship program. Pewlett-Hackard is in pretty dire straits when it comes to their workforce. Even if only half of eligible employees retire, they still need to be in the position to hore almost 50,000 new positions to fill those roles. They do not have the necessary resources, currently, to handle that transition.
