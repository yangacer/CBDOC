OUTLINE
====
__I. Client__
----
1. CloudAgent

	1.1 Installation/Building
	
		- Install Binary Packages
		- Build from Source
			UNIX
			Windows
			
	1.2 Hello CloudBase
	
	1.3 Basic Operations
	
		- Put
		- Get
		- List
		- Delete
		
	1.3 Example Application - Cloudback
	
__II. Server__
----
1. CloudGate

	1.1 Installation/Building
	
	1.2 Run
	
2. Cloudie

	1.1 Installation/Building
	
	1.2 Run

CONTENT
====	

I. Client
---

###1.1 Installation/Building


- Install Binary Packages
In case of using binary packages, there is nothing different in Windows and UNIX. Thus, just go to the download page! After extracting the package you downloaded, you should find two directories Debug and Release that contains debug version 
headers/libs and release version headers/libs, respectively.
	
- Build from Source
Source code can be obtained via SVN. Again, see download page to get it. Notice followed build procedure reuqires CMake and GCC/Visual C++ installed in your environment. 

UNIX:

	cd CloudAgentRepo
	sh install-bsd.sh PathYouLikeToInstall	
WIN: 
**You need to run followed command in "Command Prompt" provided by Visual Strudio.**

	cd CloudAgentRepo
	install-win.bat PathYouLikeToInstall

	