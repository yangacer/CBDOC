#OUTLINE

##__I. Introduction__

Big Picture
![CloudBase-arch.png](http://140.123.101.146/NUWeb_Site.cht/show_page.php?file_path=dir_024/dir_004/dir_001/file_001.png "CloudBase Architecture")

##__II. Client__

1. CloudAgent

	1.1 Installation/Building
	
		- Install Binary Packages
		- Build from Source
			UNIX
			Windows
			
	1.2 Hello CloudBase
	
	1.3 Meta Fields
	
	1.4 More Examples
	
	1.5 Example Application - Cloudback
	
##__III. Server__

1. CloudGate

	1.1 Installation/Building
	
	1.2 Run
	
2. Cloudie

	1.1 Installation/Building
	
	1.2 Run

#CONTENT

##I. Client

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
	
	// ---- HelloCB.c -----
	//
	// The only header you need
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

To compile this example:
	
	gcc HelloCB.c -I/usr/local/include -IPathYouLikeToInstall/include \ 
		-L/usr/local/lib -LPathYouLikeToInstall/lib \
		-lcloudagent -lACE -lGUtils -lMD5CC

	(Pretty much, unh? That's another story we'll never tell you.)

For windows users
	
	-I Include search path.
	-L Library search path.
	-l Library name to linked.

If you are family with CMake build script, we recommand you to take a look
MakeLists.txt located in CloudAgentRepo/example/c-binding.

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

###1.3 Meta Fields

Meta fields are assembled as 

> field_name:value;[field_name:value;[...]]
> e.g. "name:/myDoc/report.txt;desc:Seminar-Report;tag:report,seminar"

####1. Must Have Fields

- name The path name of a file. You can use path delimiters either '\' or '/'.
  The CloudBase provides prefix name listing similar to a tradition file system.
  See basic operation - list for more information.

- size Size of a file. Notice the CloudAgent client library will fill out this
  field automatically. When one specify this field as zero, only meta fields 
  will be created in CloudGate.

- owner [PRESERVED]

####2. Optional Fields

- desc Description string.

- type [PRESERVED]

- ctime Created time of format yyyyMMddhhmmss.

- mtime Modified time of format yyyyMMddhhmmss.

- tzone Time zone of format [-]offset to UTC.

- fattr File attributes.

- allowed [PRESERVED].

- tag Tag strings of format tag[','tag[...]].

####3. Automatic Generated Fields

- oid ObjectID.

- md5 MD5 digest.

