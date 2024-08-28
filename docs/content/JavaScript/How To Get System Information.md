Ref: 
[Note](obsidian://open?vault=Obsidian%20Vault&file=Tech%20News%2FThe%20JavaScript%20Open-Source%20Ebook)

'os' Module 
- retrieve info about environment used for running Node.js script or application
- method : 
	- cpus(): returns the list of CPUS available in your environment
	- version(): return the version of the Operating System in use 
	- platform(): return the name of the platform 


Test Using 'os' Module 
 - Install Node.js : [https://nodejs.org/](https://nodejs.org/)


// test.js 
```js
const os = require('os');  
  
// Getting the operating system platform  
console.log('Platform:', os.platform());  
  
// Getting the operating system architecture  
console.log('Architecture:', os.arch());  
  
// Getting information about the CPUs  
// console.info(cpu.model + ' speed: ' + cpu.speed)  
console.log('CPUs:', os.cpus());  
  
  
// Getting total system memory  
console.log('Total memory:', os.totalmem());
```

![[Screen Shot 2023-10-30 at 2.32.34 PM.png]]