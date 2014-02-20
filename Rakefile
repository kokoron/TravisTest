require "rubygems/version"
require "rake/clean"

# Application info
APP_NAME = "TravisTest"
SDK = "iphoneos"
WORKSPACE_DIR = File.expand_path("#{APP_NAME}.xcworkspace")
SCHEME = "TravisTest"
CONFIGURATION = "Release"
INFO_PLIST = File.expand_path("#{APP_NAME}/#{APP_NAME}-Info.plist")
 
DEVELOPER_NAME=""
PROFILE_NAME = ""
PROFILE_DIR = ""
PROVISIONING_PROFILE = ""
KEYCHAIN_NAME = ""
KEY_PASSWORD = ""

# Test info
DESTINATIONS = ["name=iPhone Retina (4-inch),OS=7.0",
                "name=iPhone Retina (4-inch 64-bit),OS=7.0"]
 
# Build paths
BUILD_DIR = File.expand_path("build")
APP_FILE = File.expand_path("#{BUILD_DIR}/#{CONFIGURATION}-iphoneos/#{APP_NAME}.app")
IPA_FILE = File.expand_path("#{BUILD_DIR}/#{APP_NAME}.ipa")
DSYM_FILE = File.expand_path("#{BUILD_DIR}/#{CONFIGURATION}-iphoneos/#{APP_NAME}.app.dSYM")
DSYM_ZIP_FILE = File.expand_path("#{BUILD_DIR}/#{APP_NAME}.app.dSYM.zip")


CLEAN.include(BUILD_DIR)
CLOBBER.include(BUILD_DIR)

task :default => [:build]
 
task :setup do
  system 'bundle install'
  system 'pod install'
end
 
task :test do |t|
  DESTINATIONS.each do |destination|
    options = {
      sdk: 'iphonesimulator',
      workspace: WORKSPACE_DIR,
      scheme: SCHEME,
      configuration: 'Debug',
      destination: destination
    }
    options = join_option(options: options, prefix: "-", seperator: " ")
    sh "xcodebuild #{options} test | xcpretty -tc; exit ${PIPESTATUS[0]}"
  end
end

desc "Build application"
task :build do |t|
  options = {
    sdk: SDK,
    workspace: WORKSPACE_DIR,
    scheme: SCHEME,
    configuration: CONFIGURATION
  }
  options = join_option(options: options, prefix: "-", seperator: " ")
  settings = {
    OBJROOT: BUILD_DIR,
    SYMROOT: BUILD_DIR,
    CODE_SIGN_IDENTITY: DEVELOPER_NAME,
    PROVISIONING_PROFILE: ""
  }
  settings = join_option(options: settings, prefix: "", seperator: "=")
  sh "xcodebuild #{options} #{settings} clean build | xcpretty -c; exit ${PIPESTATUS[0]}"
end


def join_option(params = {})
  options = params[:options]
  prefix = params[:prefix]
  seperator = params[:seperator]
  _options = options.map { |k, v| %(#{prefix}#{k}#{seperator}"#{v}") }
  _options = _options.join(" ")
end

desc "Print the current version"
task :version => 'version:current'