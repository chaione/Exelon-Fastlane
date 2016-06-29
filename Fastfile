require "../Configuration/AppConfiguration"

before_all do
	#bundle_install
	#carthage(command: 'build', platform: 'iOS')
	build_number = number_of_commits
	increment_build_number(build_number: build_number)
	commit_version_bump(message: "Version Bump", xcodeproj: settings[:xcode_project])
end

def pushToHockey(appName, token)
	puts 'uploading: ./build/#{appName}.ipa**************'
	# hockey(
	# 	api_token: token,
	# 	ipa: './build/#{appName}.ipa',
	# 	notes: "Changelog"
	# 	)
end

lane :buildQA do
	gym(
		configuration: "QA-ChaiOne",
		scheme: "ChaiOne",
		silent: true,
		clean: true,
		use_legacy_build_api: true,
		output_directory: "./build/", # Destination directory. Defaults to current directory.
		output_name: "#{settings[:project_name]}.ipa"	# specify the name of the .ipa file to generate (including file extension)
		)

	pushToHockey(settings[:project_name], settings[:hockey_QA_Token])
end

lane :Nightly do
	bundle_install
	carthage(command: 'build', platform: 'all')

	gym(
		configuration: "QA-ChaiOne",
		scheme: "ChaiOne",
		silent: true,
		clean: true,
		use_legacy_build_api: true,
		output_directory: "./build/", # Destination directory. Defaults to current directory.
		output_name: "#{settings[:project_name]}.ipa"	# specify the name of the .ipa file to generate (including file extension)
		)

	pushToHockey(settings[:project_name], settings[:hockey_QA_Token])
end

lane :buildProd do
	gym(
		configuration: "Release-ChaiOne",
		scheme: "ChaiOne",
		silent: true,
		clean: true,
		use_legacy_build_api: true,
		output_directory: "./build/", # Destination directory. Defaults to current directory.
		output_name: "#{settings[:project_name]}.ipa"	# specify the name of the .ipa file to generate (including file extension)
		)

	# TODO: Need Hockey API_TOKEN for production
	pushToHockey(settings[:project_name], settings[:hockey_PROD_Token])
end