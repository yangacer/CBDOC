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


- Install Binary Packages: 
  In case of using binary packages, there is nothing different in Windows and UNIX. 
  Thus, just go to the download page! After extracting the package you downloaded, 
  you should find two directories Debug and Release that contains debug version 
  headers/libs and release version headers/libs, respectively.
	
- Build from Source: 
  Source code can be obtained via SVN. Again, see download page to get it. Notice 
  followed build procedure reuqires CMake and GCC/Visual C++ installed in your 
  environment. 

UNIX:

	cd CloudAgentRepo
	sh install-bsd.sh PathYouLikeToInstall	
WIN: 
**You need to run followed command in "Command Prompt" provided by Visual Strudio.**

	cd CloudAgentRepo
	install-win.bat PathYouLikeToInstall

###1.2 Hello CloudBase

	#include "CloudAgent/CWrapper.h"

	int 
	main(int argc, char** argv)
	{
		// See server list to get online servers' IP and port.
		// You can find name.scma in PathYouLikeToInstall/bin/
		//
		// Initiate CloudAgent
		struct CloudAgent *client = CloudAgent_init("localhost", 10103, "name.scma");	

		char *feedback = 0;
		int feedback_size = 0;
		int ret_code_or_oid = 0;
		
		// Put file (argv[1]) with metadata (argv[2])
		// Note the library allocate memory for placing feedback string
		ret_code_or_oid = putObject_feedback(client, argv[2],  argv[1], &feedback, &feedback_size);

		// Check if any error encountered
		if(ret_code_or_oid < 0){
			printf("error encountered:\n%s", last_error(client));
			printf("file name: %s\n", argv[1]);
			return 0;
		}

		// Echo feedback message
		printf("Feedback:\n");
		fwrite(feedback, feedback_size, 1, stdout);

		// Free memroy allocated to feedback message
		if(feedback)
			free(feedback);
		
		// Cleanup 
		CloudAgent_fini(&client);

		return 0;	
	}

After executing ./HelloCB myfile 'name:/HelloCB.file;owner:newby'

	Feedback:
	@
	@GAIS_REC:
	@keywords:
	@mtime:20110406204122
	@tag:
	@name:HelloCB.file
	@type:
	@desc:
	@tzone:8
	@index:0
	@oid:1
	@ctime:20110407220548
	@fattr:
	@size:172
	@allowed:
	@md5:56ae2855587cab9a1db426b20782be5c
	@owner:newby

