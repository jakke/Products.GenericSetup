Products.GenericSetup Changelog
===============================

1.7.2 (unreleased)
------------------

- TBD


1.7.1 (2012-02-28)
------------------

- Restored the ability to make the setup tool use only import / export
  steps explicitly called out by the current profile, ignoring any which
  might be globally registered.  This is particularly useful for configuring
  sites with baseline profiles, where arbitrary add-on steps are not only
  useless, but potentially damaging.


1.7.0 (2012-01-27)
------------------

- While importing toolset.xml, print a warning when the class of a
  required tool is not found and continue with the next tool.  The
  previous behaviour could break the install or uninstall of any
  add-on, as the missing class may easily be from a different
  unrelated add-on that is no longer available in the zope instance.

- Exporters now explicitly only understand strings. The provided
  registry handlers encode and decode data automatically to and from
  utf-8. Their default encoding changed from None to utf-8.
  If you have custom registry handlers, ensure that you encode your unicode.
  Check especially if you use a page template to generate xml. They return
  unicode and their output must also encoded.
  If you choose to encode your strings with utf-8, you can be sure that
  your code will also work with GenericSetup < 1.7


1.6.4 (2011-10-31)
------------------

- Added three missing explicit InitializeClass calls.

- Adjust test assertions in `test_differ` to cope with changes to the diff
  library done in Python 2.7.

- LP #850665:  match permission binding to method name.

- LP #602989:  export / import new MailHost properties, `smtp_queue` and
  `smtp_queue_directory`.

- ZCML: Don't require description for the `upgradeDepends` directive.

- PythonScript handler: Newlines are now normalized during import.

- No longer rely on `bobobase_modification_time`.

- TarballImportContext: Fixed type of 'getLastModified' return value.

- LP #388380:  added docstrings to
  ``Products.GenericSetup.tool.manage_deleteImportSteps``
  and ``manage_deleteExportSteps``, fixing broken TTW deletion.

- Avoid using DateTime values for file system time stamps in tests.

- If profiles get imported multiple times within a second, generate new
  ids for each profile. This removes spurious failures in fast tests
  of packages that use GenericSetup.


1.6.3 (2011-04-02)
------------------

- Fixed crash at export when a node had None value.

- Refactored global registries to use global named utilities.

- Fixed the profile_id UnboundLocalError in the upgradeDepends directive when
  import_profile is not None.

- Removed ``five.formlib`` dependency. ``zope.formlib`` is now used directly.

- tool: 'listContextInfos' now returns profile infos sorted by type and title.
  This makes it easier to select profiles on the "Import" and "Comparison" tab. 

- Property import/export: Fixed two 'date' property issues.
  Naive 'date' values are now exported without time zone. And purging
  non-deletable 'date' properties is fixed.

- Export content objects whose 'manage_FTPget' returns a custom iterator
  with 'file' and 'size' properties.  https://bugs.launchpad.net/bugs/722726

- Property import: Fixed 'lines' and 'tokens' import.
  Modifying sequences without adding new elements was broken.

- Toolset import: Support replacement of subclassed tools.


1.6.2 (2010-08-12)
------------------

- testing: Removed broken run function.
  Unit test modules are no longer directly executable.

- DateTime 2.12.5 does away with a special case representing 
  DateTime values for midnight (00:00:00) without their time and 
  time zone values. So DateTimes formerly rendered as 
  ``2010/01/01`` in the UTC timezone now render as 
  ``2010/01/01 00:00:00 UTC``. The XML used for testing has been 
  changed to reflect this change. Since the change is only cosmetic, 
  nothing changes with respect to importing Time-less date values.

- Toolset import: Don't ignore errors in ImmutableId._setId().


1.6.1 (2010-07-04)
------------------

- Use the standard libraries doctest module.

- Deal with deprecation warnings for Zope 2.13.

- Fixed bug which broke the tool upgrade tab after running an upgrade step
  which used ``None`` as its destination version. 
  https://bugs.launchpad.net/bugs/553338


1.6.0 (2010-03-08)
------------------

- When exporting a tarball, make the directory entries executable.

- When the MailHost smtp_uid or smtp_pwd settings are None, export
  them as empty string, to avoid an AttributeError during export.

- Don't try to reinitialize a tool if an instance of the tool exists but the
  desired tool class was not resolvable. Show a warning instead of failing.

- Removed backwards compatibility code for no longer supported Zope versions.


1.6.0b1 (2010-01-31)
--------------------

- Require at least Zope 2.12.3 and use the optional five.formlib extension.

- Fixed bug in the export code of persistent utilities with explicit OFS ids.

- Prefer the class over the five:implements ZCML directive.


1.5.0 (2010-01-01)
------------------

- The components handler now ensures there is a valid component
  registry (not the global registry) before importing or exporting.

- Make sure the Import ZMI tab does not blow up if no base profile
  has been selected, and make it a little more user-friendly.

- Log if the components handler runs and has nothing to import.

- Use five.formlib in favor of Products.Five.formlib if it is available.

- Removed testing dependency on zope.app.testing.ztapi.

- tarball contexts: Fixed export and import of directory structures.


1.5.0b1 (2009-09-25)
--------------------

- LP #388380:  removed obsolete STX docs from the package directory.

- Made export / import features for old-school TextIndex (removed in Zope 2.12)
  conditional.

- Added support for import / export of subscribers from component registry.

- Adapter removal.

- Fix utility removal so utility is not added when it is missing from the
  local component registry.

- Fixed component handler to use ``for`` not ``for_`` in adapter directive. 
  To support import of existing profiles ``for_`` is used as a fallback.

- Changed testing.py to directly load zope.traversing's ZCML instead of going
  via the Five traversing.zcml BBB shim.

- Added new feature to the component handler. For factory based utilities you
  can now specify an additional id. All factory based utilities will now by
  default be added to the site manager (being an ObjectManager itself) as an
  object and this persistent object is registered as the utility. On removal
  both the registration and the object are removed. The new id argument is
  used to specify the id of the object as set via `__name__`. This change
  makes these utilities introspectable in the ZMI and clearly separates the
  persistent object and utility registration aspect.

- Adjusted TarballImportContext to work with Python 2.6's tarfile module.

- Cleaned up / normalized imports:

  o Don't import from Globals;  instead, use real locations.

  o Make other imports use the actual source module, rather than an
    intermediate (e.g., prefer importing 'ClassSecurityInfo' from
    'AccessControl.SecurityInfo' rather than from 'AccessControl').

  o Avoid relative imports, which will break in later versions of Python.

- events: Added 'handleProfileImportedEvent' subscriber.
  After a full import it updates 'last version for profile'.

- UpgradeSteps: Improved listUpgradeSteps behavior.
  If versions and checker are specified for a step, the checker is used as an
  additional restriction.

- Component registry import: Add the ability to unregister a component
  by specifying the "remove" attribute inside a utility node.
  (https://bugs.launchpad.net/zope-cmf/+bug/161728)

- Property import/export tests: Add testing for non-ASCII properties.
  (https://bugs.launchpad.net/zope-cmf/+bug/202356)
  (https://bugs.launchpad.net/zope-cmf/+bug/242588)

- Add 'IChunkedImportContext' interface, allowing RAM-efficient chunked
  reads of large files, and implement for 'DirectoryImportContext'.
  (https://bugs.launchpad.net/zope-cmf/+bug/259233)

- Add <genericsetup:upgradeDepends> ZCML tag; defines a specialized upgrade
  step that re-applies one or more import steps from a GS profile during
  an upgrade process

- Add 'IChunkedExportContext' interface, allowing RAM-efficient chunked
  writes of large files, and implement for 'DirectoryExportContext'.
  (https://bugs.launchpad.net/zope-cmf/+bug/257365)

- Provide default for dependencies when processing metadata.xml, to
  avoid a KeyError.
  (https://bugs.launchpad.net/zope-cmf/+bug/255301)

- Handle utility factories cleanly if zope.component >=3.5.0 is used.
 
- tool and utils: Removed deprecated code.

- Update PropertyManagerHelpers to make it possible to remove elements from a
  property by adding a remove="True" attribute to the element. This can
  also be used to reorder elements since new elements are always added
  at the end of the list.

- Made PropertyManagerHelpers class work for non-PropertyManager objects

  o Derived classes can supply a '_PROPERTIES' scehma, which is then used
    to mock up a temporary propertysheet for the object.  The adapter's 
    methods ('_extractProperties', '_purgeProperties', '_initProperties')
    then run against that propertysheet.

- Added logic to respect the destination of upgrade steps when determining
  their applicability. 

- Enhanced the readability of the upgrades tab on the tool.

- Use the parse_version function from pkg_resources to normalize versions
  before comparing them inside the upgrade code. This ensures pre-release
  versions are handled correctly. Also use the normalize code when sorting
  versions on the tools ZMI upgrades page.

- Fixed the upgrade step directive schema. Description is not required.

- Introduced a new IComponentsHandlerBlacklist interface. You can register
  named utilities for it and provide sequences of interfaces which should
  not be handled by the standard components registry adapter. This allows
  more specialized export/import handlers to take full control over the
  components they care about.

- When loading multiple profiles reload the list of steps to use after
  each import. https://bugs.launchpad.net/zope-cmf/+bug/213905


1.4.5 (2009-06-20)
------------------

- events: Added 'handleProfileImportedEvent' subscriber.  After a full import,
  it updates 'last version for profile'.  (Backported from trunk)

- Added a for_=None parameter to tool.py:listProfileInfo to have the same
  signature as registry.py:listProfileInfo, so profiles can be filtered by 
  interfaces.


1.4.4 (2009-05-15)
------------------

- Make sure that 'manage_createSnapshot' returns something to the browser
  when it's done, preventing an apparent hang.
  (http://dev.plone.org/plone/ticket/8452, 
  https://bugs.launchpad.net/zope-cmf/+bug/161730)

- Fixed invalid XML for the "Import' tab so it doesn't break when rendered
  with Chameleon.


1.4.3 (2009-04-22)
------------------

- Recognize acquisition-wrapped components as being of the right underlying
  type when testing for replacement during import.  
  (https://bugs.launchpad.net/zope-cmf/+bug/365202)

- Don't fail when a sub-item cannot be adapted after creation when
  importing a folder.  (https://bugs.launchpad.net/zope-cmf/+bug/300315)

- Avoid even an explicit purge of the rolemap if no XML file is present
  in a given context.  (https://bugs.launchpad.net/zope-cmf/+bug/279294)

- Changed upgrade logic to set the current version after an upgrade to the
  destination version of the last step run, instead of the current profile
  version.


1.4.2.2 (2008-09-22)
--------------------

- Packaging update:  version of 1.4.2.1 said '1.4.2'.


1.4.2.1 (2008-09-22)
--------------------

- Packaging update:  version of 1.4.2 said '1.4.2dev'.


1.4.2 (2008-09-22)
------------------

- Add 'IChunkedImportContext' interface, allowing RAM-efficient chunked
  reads of large files, and implement for 'DirectoryImportContext'.
  (https://bugs.launchpad.net/zope-cmf/+bug/259233)

- Add 'IChunkedExportContext' interface, allowing RAM-efficient chunked
  writes of large files, and implement for 'DirectoryExportContext'.
  (https://bugs.launchpad.net/zope-cmf/+bug/257365)

- Update local component registry importer to prevent it from overwriting
  existing utilities if they are already of the correct type

- Property import/export tests: Fix and test for non-ASCII properties.
  (https://bugs.launchpad.net/zope-cmf/+bug/202356)
  (https://bugs.launchpad.net/zope-cmf/+bug/242588)

- Provide default for dependencies when processing metadata.xml, to
  avoid a KeyError.
  (https://bugs.launchpad.net/zope-cmf/+bug/255301)

- Update PropertyManagerHelpers to make it possible to remove elements from a
  property by adding a remove="True" attribute to the element. This can
  also be used to reorder elements since new elements are always added
  at the end of the list.


1.4.1 (2008-05-27)
------------------

- When loading multiple profiles reload the list of steps to use after
  each import. https://bugs.launchpad.net/zope-cmf/+bug/213905


1.4.0 (2008-03-23)
------------------

- Make getProfileImportDate handle situations where one object's id
  is a prefix of another id.


1.4.0-beta (2008-02-07)
-----------------------

- During object manager imports do not throw an error when
  trying to remove an object that was already removed.

- utils: Added MarkerInterfaceHelpers.

- Added default values to the registerProfile ZCML directive.

- Add a ZMI interface to find and remove invalid steps from the
  persistent registries.

- All GenericSetup import and export steps are now registered globally.

- Remove duplicated test (https://bugs.launchpad.net/zope-cmf/+bug/174910)

- Don't create empty 'import_steps.xml' and 'export_steps.xml' files.

- Fix relative paths for profile dependencies.

- Add support for context dependencies in profiles.

- Deprecate the version field for import steps.

- Deprecate reading of version.txt to get the version for a profile.

- Fire events before and after importing.

- Use zcml to register import and export steps.


1.3.3 (2007-12-29)
------------------

- Be more careful in checking context id validity.

- tool: Fixed toolset import handler not to initialize tools again, when
  they already exist in the site.


1.3.2 (2007-09-11)
------------------

- Ignore import and export step handlers that we can not resolve.

- Restore the import context after running steps from a profile
  so we do not break on nested calls.

- components: Provide log output when purging utilities or adapters.

- components: Fixed an undefined variable name in a log message.


1.3.1 (2007-08-08)
------------------

- components: correct the object path for the site root to be the
  empty string.

- components: Made output more diff friendly.

- utils: Added warnings to old code.
  ImportConfiguratorBase and ExportConfiguratorBase will become deprecated
  as soon as GenericSetup itself no longer uses them. HandlerBase is now
  deprecated.

- components: Added 'components_xmlconfig.html' form.
  This view allows to inspect and edit component registrations. It is also
  available under the ZMI tab 'manage_components'.


1.3 (2007-07-26)
----------------

- components: Removed non-functional support for registering objects in
  nested folders. We only support objects available in the component
  registry's parent now. The component registry needs to be either
  acquisition wrapped or have a __parent__ pointer to get to the parent.


1.3-beta (2007-07-12)
---------------------

- Guard against situations where encoded text may be compared by the
  differ.
  (http://www.zope.org/Collectors/CMF/471)

- Extend the ZCatalog import/export mechanism to allow removal of 
  metadata columns in addition to adding them.
  (http://www.zope.org/Collectors/CMF/483)

- Made sure we register Acquisition free objects as utilities in the
  components handler.

- Profiles now support version numbers; setup tool tracks profile
  versions during upgrades.

- Added support for nested 'upgradeStep' directives; expanded upgrade
  step registry into a real registry object and not just a dictionary.

- Added support for 'metadata.xml' in the profile (read during
  profile registration) to register profile description, version,
  and dependencies.

- Deprecated runImportStep and runAllImportSteps in favor of
  runImportStepFromProfile and runAllImportStepsFromProfile.

- Merged CPS's upgradeStep ZCML directive, w/ corresponding tool support.

- Added a "last imported" date to the list of extension profiles,
  and to the baseline profile.

- Renamed the "Properties" tab to "Profiles".

- Removed the 'create_report' decoy in the ZMI view methods:  there was
  never any UI for passing any value other than the default, anyway, and
  the report objects are too useful to omit.

- Refactored the "Properties" tab to separate baseline profiles from
  extension profiles, marking the option to reset the baseline as
  potentially dangerous for sites which already have one.  Allow
  importing one or more extension profiles directly (all steps) from the 
  "Properties" tab.

- No longer read the toolset xml and update the toolset regustry on
  import context change.  Doing this only during the toolset step import
  should be sufficient.

- testing: The test base classes no longer set up any ZCML.
  This change is not backwards compatible. If you are using these base
  classes for testing custom handlers, you have to add the necessary ZCML
  setup and tear down. Using test layers is recommended.

- Added support for importing-exporting Zope 3 component registries
  by folding in Hanno Schlichting's GSLocalAddons product.


1.2-beta (2006-09-20)
---------------------

- tool:  Added support for uploading a tarball on the "Import" tab
  (i.e., one produced on the export tab).

- docs: Added SampleSite demo product.

- ProfileRegistry: Added 'registerProfile' ZCML directive.
  Using the old registerProfile method in initialize() is now deprecated.
  See doc/profiles.txt for details.

- ProfileRegistry: 'product' should now be the module name.
  For backwards compatibility 'product' is still first looked up in
  Products before searching the default module search path.

- ZCTextIndex handler: Fixed 'indexed_attr' import.
  (http://www.zope.org/Collectors/CMF/436)

- docs: Added 'Registering Profiles' section to profiles.txt.

- Added support for PageTemplate import/export, modeled closely after
  existing PythonScript support

- The dependency sorting was highly reliant on steps being added in the
  right order to work. If import step A depends on import step B which 
  depends on step C, and step C gets processed early, and they were 
  processed in the order A, C, B, then the dependency order would be 
  incorrect. This is now fixed by keeping tack of steps with unresolved
  dependencies, and trying again after inserting everything else.


1.1 (2006-04-16)
----------------

- ZCatalog handler: Implemented the 'remove' directive for indexes.
  This allows to write extension profiles that remove or replace indexes.

- getExportStepRegistry had the wrong security declaration


1.1-beta2 (2006-03-26)
----------------------

- No changes - tag created to coincide with CMF 2.0.0-beta2


1.1-beta (2006-03-08)
---------------------

- Allowed subclasses of DAVAwareFileAdapter to override the filename
  in which the file is stored.

- Added a doc directory including some basic documentation.

- Made GenericSetup a standalone package independent of the CMF

- Added ``for_`` argument to profile registry operations.
  A profile may be registered and queried as appropriate to a specific
  site interface;  the default value, 'None', indicates that the profile
  is relevant to any site.  Note that this is essentially an adapter
  lookup;  perhaps we should reimplement it so.

- Forward ported changes from GenericSetup 0.11 and 0.12 (which were
  created in a separate repository).

- A sequence property with the purge="False" attribute will not be
  purged, but merged (the sequences are treated as sets, which means
  that duplicates are removed). This is useful in extension profiles.

- Don't export or purge read-only properties. Correctly purge
  non-deletable int/float properties.

- Correctly quote XML on export.


1.0 (2005-09-23)
----------------

- CVS tag:  GenericSetup-1_0

- Forward-ported i18n support from CMF 1.5 branch.

- Forward ported BBB for old instances that stored properties as
  lists from CMFSetup.

- Forward ported fix for tools with non unique IDs from CMFSetup.


0.12 (2005-08-29)
-----------------

- CVS tag:  GenericSetup-0_12

- Import requests now create reports (by default) which record any
  status messages generated by the profile's steps.


0.11 (2005-08-23)
-----------------

- CVS tag:  GenericSetup-0_11

- Added report of messages generated by import to the "Import" tab.

- Consolidated ISetupContext implementation into base class,
  'SetupContextBase'.

- Added 'note', 'listNotes', and 'clearNotes'  methods to ISetupContext,
  to allow plugins to record information about the state of the operation.


0.10 (2005-08-11)
-----------------

- CVS tag:  GenericSetup-0_10

- Added TarballImportContext, including full test suite.


0.9 (2005-08-08)
----------------

- CVS tag:  GenericSetup-0_9

- Initial version, cut down from CMFSetup-1.5.3
