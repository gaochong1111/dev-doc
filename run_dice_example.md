# RUNNING EXAMPLES
## command line arguments
check
--model-input-files
model_file
--property-input-files
property_file
--model-input-type
prism
--plugin
plugin_class_file_list

## debug process
1. parse arguments 
    get 17 plugin-path List<String>

2. loadPlugins: get List<Class<? extends PluginInterface> >
- new PluginLoader: 
    (1) List<String> -> List<Path>
    (2) init classLoader: URLClassLoader
    (3) loadPlugins: List<Plugin>
        Plugin: {path, name, dependencies, classes}
        loadPlugin:
            setPath
            setName, addDependencies (addManifestInfomation)
                $Path/META-INF/MANIFEST.MF -> java.util.jar.Manifest
            checkPluginSanity: 
                check the order of plugin
            loadPluginClasses: from classes.txt
                add():classes
- PluginLoader.getPluginInterfaceClasses
    get List<Class<? extends PluginInterface> >

3. StartInConsole
- epmc.jani.interaction.commandline.StartInConsoleJaniInteractionNoJani
    process(args, plugins)
        prepareOption
            set options #TODO
        new CommandTask
        command.execute()
            new RawModel
                modelFilenames
                propertyFilename
            Analyse.execute(rawModel, options)
                save options
                ===========
                processAfterServerStart
                    epmc.jani.interaction.plugin.AfterServerStartJANIInteraction
                processBeforeModelCreations
                parseModel
                processAfterModelCreations
                new ModelChecker(model)
                new CommandTask
                command.executeInServer()
                    modelChecker.check()
                processAfterCommandExecution()
                ===========
                recovery options
- epmc.jani.interaction.commandline.StartInConsoleJaniInteractionJani
    process()
    Problem ???

