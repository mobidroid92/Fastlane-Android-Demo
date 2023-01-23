
  desc "Upload new staging apk to AppCenter"
  lane :staging_build_to_app_center do

    changelog = prompt(text: "Release Notes: ", multi_line_end_keyword: "END")

    ticketsIDs = prompt(text: "Please enter tickets IDs comma separated to change it's status to be Ready For QA on JIRA. Like: SHAR-1,SHAR-2,....")
    ticketsIDsArray = ticketsIDs.split("," , -1)

    branchName = "<Your Branch Name>”

# 	Switch to current sprint branch
	git_switch_branch(branch: branchName)

# 	If current branch not current sprint branch make exception
	ensure_git_branch(branch: branchName)

# 	Pull latest changes
	git_pull

	appPath = "<Your Project Path>/app/"

	increment_version_code(
		gradle_file_path: appPath + "build.gradle"
	  )

# 	Clean project
	gradle(task: "clean")

   buildType = "<Your Build Type>" # Like Debug
   
#   Use bundle instead of assemble to generate an aab instead of apk
   taskToExecute = "assemble" + buildType
   
# 	Build signed APK
	gradle(
 	 task: taskToExecute,
 	 properties: {
  	  "android.injected.signing.store.file" => "<Your Store File>",
 	   "android.injected.signing.store.password" => "<Your Store Password>",
 	   "android.injected.signing.key.alias" => "<Your Alias>",
  	  "android.injected.signing.key.password" => "<Your Key Password>",
  	 }
	)

    filePath = appPath + "build/outputs/apk" + "/" + buildType.downcase + "/app-" + buildType.downcase + ".apk"
	appcenter_upload(
 	 api_token: "<Your API Token>",
 	 owner_name: "<Your Owner Name>",
 	 owner_type: "<Your Owner Type>",
 	 app_name: "<Your App Name>",
   file: filePath,
	 release_notes: changelog,
	 destinations: "<Your Destinations>",
 	 notify_testers: true
	)

    def jira_client
        site = "<Your Site>"
        context_path = ""
        auth_type = :basic
        username = "<Your Username>"
        password = "<Your Password>" # Or you can generate an API Auth Token and use it here.
        options = {
            site: site,
            context_path: context_path,
            auth_type: auth_type,
            username: username,
            password: password
        }
        client = JIRA::Client.new(options)
        client
    end

  	client = jira_client()
    for ticketID in ticketsIDsArray do
       	jira_transitions(
       		client: client,
       		ticket_id: ticketID,
       		matching: lambda { |transition|
                        name = transition.name
                        name == 'Ready For Test' || name == 'Ready For QA' # Change this to match your transition name
                        },
       		fail_if_no_matching: false
       	)
    end

	teams_url = "<Your Teams Webhook>"
    teams(
        title: "New Build Released",
        message: changelog,
         facts:[
          {
            "name"=>"name",
            "value"=>"value"
          }
        ],
        teams_url: teams_url
      )

end
