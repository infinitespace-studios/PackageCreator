## InfinitespaceStudios.MonoGame.Packaging

InfinitespaceStudios.MonoGame.Packaging is a set of MSBuild tool tasks which allow MonoGame DesktopGL developers to create self contained Linux and Mac distribution packages. It uses a number of other open source projects

- [Monokickstart](https://github.com/MonoGame/MonoKickstart)
- [LibZipSharp](https://github.com/grendello/LibZipSharp)

### Usage

1. Add the Nuget package InfinitespaceStudios.MonoGame.Packaging to your DesktopGL project.
2. Build the app as normal in Release mode.
3. Use the following to create a Mac Package

	msbuild <project> /p:Configuration=Release /t:BuildGamePackages /p:PackageTargetPlatforms=MacOS

This will produce an .app package in the $(OutputDir).
Note you should run this command in the same directory as the csproj for your
DesktopGL project. This will make sure the correct paths are used.

### Available Targest

- BuildGamePackages - Builds a number of Game paackages based on the value
  of `PackageTargetPlatforms`. 

- SignGamePackages - This is for `MacOS` only. It will sign the .app package
  with the code signing certificate defined by `PackageCodeSignCertificate`.
  It will also produce a .pkg file and sign that using the `PackageInstallerCertificate`.
 

### Available Properties

- PackageTargetPlatforms - Controls which packages are created. Valid values
  are `MacOS`, `Linux`, `Steam`, `Windows`, `ItchIo`.

- PackageIcon - The Icon to use for the package. It should be a decent resolution.
  This will be used as a basis for your `MacOS` app icon.
  Defaults to `$(MSBuildProjectDirectory)\Icon.png` if it exists.

- PackageName - The name of the Final package. For example `Foo.app` or `Foo.exe`. 
  Defaults to `$(AssemblyName)`.

- PackageDisplayName - The Dispay Name for the package. This is mostly for `MacOS`.
  Use this if you want to app name to be different from your Exe Name. For example
  For a exe called FooBar.exe you might want a Display Name of "Foo Bar!".
  Defaults to `$(PackageName)`.

- PackageId - The ID for the package. Again mostly for `MacOS`
  Defaults to `com.yourdomain.$(PackageName)`.

- PackageCopyright - Your copyright detalts.
  Defaults to `Copyright © You This Year`.

- ContentDirectory - The location to pick up the built content.
  Defaults to `$(OutputPath)Content`.

- OutputDirectory - The output path for the final packages.
  Defaults to `$(OutputPath)`.

- PackageSourceDirectory - The location to pick up the binaries for the package. 
  Note it will only pick up the files in the directory. Sub directories will
  be ignored. If you have additional files make use of the `PackageAdditionalAssemblies`
  property.
  Defaults to `$(OutputPath)`.


### Advanced Properties

- IncludeMcsInPackage - Controls if a mkbundled version of `mcs` (C# compiler) is included
  in the app packages. This only applies to Unix based packages. This should only be used
  if you find you need the compiler when deserializing data from disk. 

- PackageAdditionalAssemblies - Allows you to speficy additional assemblies to include into 
  the final package.

- PackageInstallerCertificate - The name of the Installer Certificate to use to sign
  the .pkg installer package.

- PackageCodeSignCertificate - The name of the Code Signing Certificate to use to sign 
  the .app bundle. 

- PackageCustomInfoPlist - Allows the developer too provide there own `Info.plist` for 
  `MacOS` packaging. If not provided a default one will be created.
