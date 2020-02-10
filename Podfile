plugin 'cocoapods-pod-merge'

# Uncomment the next line to define a global platform for your project
target 'GTMSupport_iOS' do
  platform :ios, '9.1'
  pod 'Firebase/Performance'
  pod 'GoogleTagManager', '~> 6.0'
  
end

post_install do |installer|
  sharedLibrary = installer.aggregate_targets.find { |aggregate_target| aggregate_target.name == 'Pods-GTMSupport_iOS' }
  installer.aggregate_targets.each do |aggregate_target|
    puts "Aggfregate target: #{aggregate_target.name}"
    puts "Shared library: #{sharedLibrary}"
    if aggregate_target.name == 'Pods-GTMSupport_iOS'
      aggregate_target.xcconfigs.each do |config_name, config_file|
        sharedLibraryPodTargets = sharedLibrary.pod_targets
        aggregate_target.pod_targets.select { |pod_target| sharedLibraryPodTargets.include?(pod_target) }.each do |pod_target|
          pod_target.specs.each do |spec|
            puts "spec: #{spec.name}"
            frameworkPaths = unless spec.attributes_hash['ios'].nil? then spec.attributes_hash['ios']['vendored_frameworks'] else spec.attributes_hash['vendored_frameworks'] end || Set.new
          frameworkNames = Array(frameworkPaths).map(&:to_s).map do |filename|
            extension = File.extname filename
            File.basename filename, extension
          end
          puts "Config file #{config_file}"
          frameworkNames.each do |name|
            puts "Framework name: #{name}"
            if name == "GoogleUtilities"
              puts "Removing #{name} from OTHER_LDFLAGS"
              config_file.frameworks.delete(name)
              config_file.libraries.delete("name")#<- What I insert
              #config_file.headers.delete("#{name}")#<- What I insert
            end
          end
          if spec.name == "Protobuf"
            puts "Removing #{spec.name} from OTHER_LDFLAGS"
            config_file.frameworks.delete(spec.name)
            config_file.libraries.delete("#{spec.name}")#<- What I insert
            #config_file.headers.delete("#{spec.name}")#<- What I insert
          end
          if spec.name == "nanopb"
            puts "Removing #{spec.name} from OTHER_LDFLAGS"
            config_file.frameworks.delete(spec.name)
            config_file.libraries.delete("#{spec.name}")#<- What I insert
            #config_file.headers.delete("#{spec.name}")#<- What I insert
          end
        end
      end
      xcconfig_path = aggregate_target.xcconfig_path(config_name)
      config_file.save_as(xcconfig_path)
    end
  end
end
end
