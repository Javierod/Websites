JavaScript
---
```javascript
//Created by Javier Rodriguez. 10/18/2017
let url = ".xlsx";					//Path to file.
let req = new XMLHttpRequest();				//Calling http protocol
req.open("GET", url, true);				//Calling file.
req.responseType = "arraybuffer";			//Setting value.

///////////////////////////////////////////////////
//FUNCTION NAME: None.				 //
//FUNCTION PURPOSE: Read data from excel fields  //
//						 //
//PARAMETERS: 1. e - onload object.		 //
//RETURN: Nothing.				 //
///////////////////////////////////////////////////
req.onload = function(e) {
	let data = new Uint8Array(req.response);			      //Setting language (EN).
	let arr = new Array();						      //Setting array.
	for(let i=0; i!=data.length; ++i)	   	                      //Transforming req characters into ASCII values.
  		arr[i] = String.fromCharCode(data[i]);
  let bstr = arr.join('');						      //Setting array.
	let workbook = XLSX.read(bstr, {type:"binary"});		      //Reading binary file.
	let first_sheet_name = workbook.SheetNames[0];			      //Defining fields.
	let worksheet = workbook.Sheets[first_sheet_name];		      //Filling array.
	dataArray = XLSX.utils.sheet_to_json(worksheet,{header:1,raw:true});  //Passing values to a local array of objects. as numbers.
                                                                              /*XLSX.utils.sheet_to_json(worksheet,{header:1})
                                                                                XLSX.utils.sheet_to_json(worksheet,{raw:true})
                                                                              */
console.log(dataArray);   	                                              //Sending array to console.
}

req.send()
  ```
code extracted from [YouTube Video](https://www.youtube.com/watch?v=9hVO9-sSOVM)

HTML file
---
```html
<!-- Created by Javier Rodriguez. 10.18.2017 -->

<!-- HTML Website -->
<!DOCTYPE html> 
<!-- HTML Structure -->
<html>
 	<!-- HTML head -->
	<head>
		<!-- ASCII code for web page -->
		<meta charset="utf-8">
		
		<!-- Library to read excel file. -->
		<script lang="javascript" src="xlsx.full.min.js"></script> 
		
		<!-- Source Code Script. -->
		<script src=""></script>
		
		<!-- Web page title -->
		<title>Reading Excel File Into JavaScript</title>
	</head>

	<!-- HTML body -->
	<body>
	</body>
</html>
```

Extra information.
---
For security reasons, we cannot access any data in a computer from a website. Hence, a server must be initiated or everything should be run from a server.
