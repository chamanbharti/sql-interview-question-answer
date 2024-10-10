  2nd Highest salary
  
1. Using the LIMIT and OFFSET Clause (MySQL, PostgreSQL)
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
2. Using the ROW_NUMBER() Window Function (SQL Server, PostgreSQL)

SELECT salary
FROM (
    SELECT salary, ROW_NUMBER() OVER (ORDER BY salary DESC) AS salary_rank
    FROM employees
) AS ranked_salaries
WHERE salary_rank = 2;
3. Using the DENSE_RANK() Window Function (SQL Server, PostgreSQL)

SELECT salary
FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank
    FROM employees
) AS ranked_salaries
WHERE salary_rank = 2;
4. Using a Subquery

SELECT MAX(salary) AS second_highest_salary
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
5. Using DISTINCT and ORDER BY

SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;  -- This is for databases that support LIMIT/OFFSET
6. Using GROUP BY

SELECT salary
FROM employees
GROUP BY salary
HAVING COUNT(*) > 1
ORDER BY salary DESC
LIMIT 1;
7. Using IN Subquery

SELECT MAX(salary) 
FROM employees 
WHERE salary IN (SELECT DISTINCT salary FROM employees ORDER BY salary DESC LIMIT 2);

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

   In Java

Map<String, Integer> map1 = new HashMap<>();

map1.put("anil", 1000);
map1.put("bhavna", 1300);
map1.put("micael", 1500);
map1.put("tom", 1600);//output
map1.put("ankit", 1200);
map1.put("daniel", 1700);
map1.put("james", 1400);

Map.Entry<String, Integer> results = getNthHighestSalary(2, map1);

System.out.println(results);

Map<String, Integer> map2 = new HashMap<>();

map2.put("anil", 1000);
map2.put("ankit", 1200);
map2.put("bhavna", 1200);
map2.put("james", 1200);
map2.put("micael", 1000);
map2.put("tom", 1300);
map2.put("daniel", 1300);

Map<Integer,List<Map.Entry<String, Integer>>> result =  map2.entrySet().stream().collect(Collectors.groupingBy(Map.Entry::getValue));

System.out.println("GroupBy:"+result);
// {1200=[ankit=1200, james=1200, bhavna=1200], 1300=[daniel=1300, tom=1300], 1000=[micael=1000, anil=1000]}

// to improve output as
// {1200=[ankit, james, bhavna], 1300=[daniel=1300, tom=1300], 1000=[micael, anil]}
Map<Integer,List<String>> result1 = map2.entrySet().stream().collect(
		Collectors.groupingBy(Map.Entry::getValue, 
		Collectors.mapping(Map.Entry::getKey, Collectors.toList())));
  
System.out.println("GroupBy:"+result1);// {1200=[ankit, james, bhavna], 1300=[daniel, tom], 1000=[micael, anil]}
System.out.println(getDynamicNthHighestSalary(2, map2));// 1200=[ankit, james, bhavna]

}
public static Map.Entry<String, Integer> getNthHighestSalary(int num, Map<String, Integer> map) {
	        return map.entrySet().stream()
	                .sorted(Collections.reverseOrder(Map.Entry.comparingByValue()))
	                .collect(Collectors.toList())
	                .get(num - 1);
	    }
     
public static Map.Entry<Integer, List<String>> getDynamicNthHighestSalary(int num, Map<String, Integer> map) {
	        return map.entrySet()
	                .stream()
	                .collect(Collectors.groupingBy(Map.Entry::getValue,
	                        Collectors.mapping(Map.Entry::getKey, Collectors.toList())
	                ))
	                .entrySet()
	                .stream()
	                .sorted(Collections.reverseOrder(Map.Entry.comparingByKey()))
	                .collect(Collectors.toList())
	                .get(num - 1);
	    }
   
   
3. #1 Find the number of employees working in each department
   Select count(*) as count,d.dept_name
   from employee e
   inner join
   department d on e.dept_id = d.id
   Group by e.dept_id
   #2 Find the number of employees working in each department and the number of employee should be more than 5
   Select count(*) as count,d.dept_name
   from employee
   inner join
   department d on e.dept_id = d.id
   Group by e.dept_id
   Having count > 5
5. 


6. 
7. 
