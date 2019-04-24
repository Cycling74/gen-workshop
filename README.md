[001-project-settings]: 	img/001-project-settings.png
[002-project-name]: 		img/002-project-name.png
[003-plugin-code]:	 		img/003-plugin-code.png
[004-export-path]:	 		img/004-export-path.png
[005-juce-build]:	 		img/005-juce-build.png
[006-xcode-build]:	 		img/006-xcode-build.png
[007-global-paths]:	 		img/007-global-settings.png
[008-vst3-path]:	 		img/008-vst3-path.png
[009-plugin-targets]:	 		img/009-plugin-targets.png

# gen-workshop
Materials for the gen~ export workshop

## Setup and Configuring JUCE to build a VST
We will be using JUCE to wrap our exported gen~ code, so there are some quick steps to follow to get everyting set up correctly. 
1. Download JUCE (see https://juce.com).

Now, if you want to be able to build VSTs as opposed to VST3 or Audio Units, there are a couple more steps. I recommend going through these steps as, from my experience, the most convenient plugin format to work with is the legacy VST2 format.

2. Download this older, special version of the VST SDK. This package has been altered slightly to build correctly with JUCE. `<Link here>`. Once you've downloaded this SDK, unzip it.

3. Open up the Projucer. In the application menu on the top left, open up Global Paths.

![Global Paths][007-global-paths]

4. Navigate down to the field for the VST3 SDK path. Browse to the location fo the VST3_SDK folder, within the VST SDK that you downloaded and unzipped in step 2.

![VST3 Paths][008-vst3-path]

5. That should be it, the setup should be complete.

## Building a VST/AU using gen~
In order to build a VST/AU using gen~, a little bit of finesse is required. We make use of the Projucer in order to create an XCode project that builds the appropriate plugin.

1. Again, make sure that the Projucer is installed.
2. In this repository directory, you will see a folder named JUCEPluginTemplate. Copy this folder any time you would like to build a new VST/AU using gen~. You should rename this folder.
3. Within the copied folder, open the file C74ProjucerGenPlugin.jucer.
4. Navigate to the Project Settings.

![Project Settings Image][001-project-settings]

5. Change the Project Name to be whatever you like. This will be the name of the plugin within any host application.

![Project Name Image][002-project-name]

6. Change the Plugin Code to be anything unique. You may also change the Plugin Manufacturer Code and Plugin Manufacturer if you like. The only rule is, no two plugins may have the same Plugin Manufacturer Code and Plugin Code. The other only rule is, the Plugin Manufacturer Code and Plugin Code must contain at least one upper-case letter. Apple's own internal plugins may violate this rule, apparently.

![Plugin Code Image][003-plugin-code]

7. Make sure that your Plugin Formats include the formats that you want to build. VST3 and AU are enabled by default, so if you want the legacy VST format you will need to include it here explicitly.

![Plugin Targets][009-plugin-targets]

8. Make whatever kind of gen~ patcher your heart desires, according to normal gen~ usage.

9. Export the gen code. Don't change the _exportname_ attribute. Be sure to export it to the Source folder of your new JUCE project. This where the JUCE project expects it to be (the header search path is configured to look for headers here).

![Export Path Image][004-export-path]

10. Build the XCode project in JUCE. This should automatically open the XCode project.

![Build JUCE Image][005-juce-build]

11. Build the XCode project. Without the VST3 SDK installed, you'll need to build an Audio Unit. This will build the actual component.

![Build XCode Image][006-xcode-build]

12. For Audio Units, this will install the component in `~/Library/Audio/Plug-Ins/Components`. VSTs will install in `~/Library/Audio/Plug-Ins/VST`

13. Unlike VSTs, Audio Units must register with the OS after being installed in order to work correctly. If you find that the component is still not registered (you run `auval -a` from the command line and don't see your component), then there is one thing you can try (I don't understand why this is a solution). Return to JUCE, change the Plugin Code to anything, rebuild the XCode project and rebuild the plugin. Run `auval -a` again and you should see the plugin under the new name. You can then change the Plugin Code back. I don't know why this helps, but it does.

14. That should be everything you need to do to build and install your plugin. From this point on, if you need to change your plugin, you need only export the code from Max and rebuild the plugin in XCode. You need not open JUCE again, unless you want to change the name of the plugin or some other configuration.
