1. Find nth highest salary in sql
   
      5 different ways
   
   Using Max()
   2nd Highest salary:
   Select max(salary) from employee
   where salary < (select max(salary) from employee);
   
   3rd Highest salary:
   
   Select max(salary) from employee
   where salary < (select max(salary) from employee where salary < (select max(salary) from employee));

   Using Limit
   
   How Limit clause works?
   
   select n from t order by n limit 3,4(3 is offset(index) & 4 is row count)
   2nd hihgest salary
   Select salary from employee
   order by salary desc
   limit 1,1;

   3rd Highest salary:
   
   Select salary from employee
   order by salary desc
   limit n-1,1;
   Where n=Number of the highest salary you want to find
   If n=2, Find second highest salary
   If n=3, Find third highest salary

   Without Limit Clause

   Select salary from employee e1
   where n-1 = (Select count(Distinct salary) from employee e2
   where e2.salary > e1.salary);
   Where n=1,2,3,...

   What is dense_rank()
   
   The dense_rank() assigns a rank to each row within a parttion or result set with no gaps in ranking values
   The rank of a row is increased by one for the distinct values
   If result set has two or more rows with the same rank value, each of these rows will be assigned the same rank.
   Select * from (Select name,salary,dense_rank() over(Order by salary desc) as salary_rank from employee) as temp
   where salary_rank=n;

   Where n=1,2,3,... highest salary you want to find
   
   
3. 
4. 


5. 
6. 
