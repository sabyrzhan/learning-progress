- ## Closures #closures
	- It is a function that can be passed as parameter to a function
	- Example:
	  ```
	  struct Student {
	      let name: String
	      var testScores: Int
	  }
	  
	  let students = [
	      Student(name: "Luke", testScores: 88),
	      Student(name: "Han", testScores: 73),
	      Student(name: "Leia", testScores: 95),
	      Student(name: "Chewy", testScores: 78),
	      Student(name: "Obi-Wan", testScores: 65),
	      Student(name: "Ahsoka", testScores: 86),
	      Student(name: "Yoda", testScores: 68)
	  ]
	  
	  let topStudentsFilter: (Student) -> Bool = { student in 
	      return student.testScores > 80
	  }
	  
	  let filteredStudents = students.filter(topStudentsFilter)
	  filteredStudents.forEach {stud in 
	      print("Stud: \(stud.name) Score: \(stud.testScores)")
	  }
	  ```
	- Trailing closure syntax is when you can omit the parameters and the closure is the last parameter of the function: #[[swift closure syntax]]
	  ```
	  let filteredStudents = students.filter { stud in  // trailing closure syntax
	  	return stud.testScores > 80
	  }
	  ```
	- `$0, $1, $2....` instead of arguments
	  This are argument references in closure bodies. In trailing closure syntax when you dont want to use the parameter names you can refer to passed parameters by sequence numbers. This also helps to write clean code with one line statement:
	  ```
	  let filteredStudents = students.filter { $0.testScores > 80 }
	  ```
	- `@escaping` #[[swift @escaping]]
	  Used when closure needs to keep in life the function that called it. For example, in network requests which latencies are unpredictable, so we need to wait for the result to process.