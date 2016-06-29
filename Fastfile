# Update these environment settings for your specific project
def settings
	{
		:slack_channel => 'exelon-dev',
		:xcode_project => 'Exelon-Dashboard.xcodeproj',
		:github_repo => 'chaione/exelon-dashboard'
	}	
end
	
def suppress_slack
	if is_ci?
		return false
	else
		return true
	end
end

before_all do
	bundle_install
	carthage(command: 'build', platform: 'iOS')
	build_number = number_of_commits
	increment_build_number(build_number: build_number)
	commit_version_bump(message: "Version Bump", xcodeproj: settings[:xcode_project])
end

lane :experiment do
	slack(slack_url: ENV['slack_url'], channel: settings[:slack_channel], message:"Testing better fastlane variables. Nothing to see here.")
end

lane :test do
	scan(scheme: "ChaiOne",
		output_directory: "report_output", 
		device: "iPad Air 2",
		slack_url: ENV['slack_url'], 
		slack_channel: settings[:slack_channel], 
		skip_slack: suppress_slack,
		slack_only_on_failure: true
		)
	xcov(scheme: "ChaiOne", output_directory: "report_output")
end

lane :inhouse do
	ensure_git_branch
	badge(shield: "#{get_version_number}-#{get_build_number}-0074A0", shield_no_resize: true)
	gym(scheme: "ChaiOne", clean: true, export_method: "enterprise")
	s3(access_key: ENV['AWSkey'], secret_access_key: ENV['AWSsecret'], bucket: 'chaione-app-store', path: '_inbox')
	set_github_release(repository_name: settings[:github_repo], 
		api_token:ENV['github_token'], 
		name: "Build ##{get_build_number}", 
		description: File.read("../RELEASE_NOTES.md"),
		tag_name: "b#{get_build_number}", 
		commitish: 'master',
		is_prerelease: true)
	if !suppress_slack
		slack(slack_url: ENV['slack_url'], channel: settings[:slack_channel])
	end
end

lane :demo do
	ensure_git_branch
	badge(shield: "#{get_version_number}-#{get_build_number}-0074A0", shield_no_resize: true)
	gym(scheme: "ChaiOne", clean: true, export_method: "enterprise")
end

after_all do |lane|
		
end

error do |lane, exception|
	if !suppress_slack
		slack(slack_url: ENV['slack_url'],
			message: exception.to_s,
			success: false,
			channel: settings[:slack_channel])
		end
end