## [[zx -  bash scipt replacement]]
	- Using
	  ```
	  $`<your command>`
	  ```
	  you can run external bash scripts, while at the same time using JavaScript
	- https://google.github.io/zx/
	- ```
	  // Running bash code
	  const list = await $`ls -la`
	  console.log(list.stdout)
	  
	  // Waiting or long running code
	  await spinner('working...', () => $`sleep 3`)
	  
	  // Using built-in question and spinner functions
	  while(true) {
	      const q = await question('Enter the number? ')
	      if (q === 'quit') {
	          break;
	      }
	      await spinner("working...", () => {
	          $`sleep 3`
	          echo`Failed to work. Please try again!\n`
	      })
	  }
	  ```