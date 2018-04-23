--- server/locale/localization_readme.txt ---


0. Change-log
=============

DOCUMENTS 4 HF1 (#1816)
-----------------------
* new keyword #$PROPERTIES_ENCODING for the definition of the encoding
  of the property-file
  e.g.  #$PROPERTIES_ENCODING =windows-1252
        If a DOCUMENTS 4 x64 UTF-8 server reads this property file, the
		server transcode the entries from windows-1252 to UTF-8


=========================
Changing the localization
=========================

1. Changes from otrisPortal51 / ELC 3.51 to otrisPortal6 / ELC 3.60
===================================================================

In former versions of otrisPortal / ELC (version < otrisPortal6 /
ELC < 3.60) localization strings like names of the standard documents
folders (e.g. Inbox / Eingangskorb) or error messages and mail templates
were defined as "System Messages" that you were able to modify in the
PortalManager, but it was difficult to change the order and translate
them.
Therefore we have made changes that these localization strings since
version otrisPortal6 / ELC 3.60 are stored in Properties-files in
the server/locale directory. These localization strings are not any
longer in the PortalManager.



2. How does it work
===================
In the server/locale directory you find the following properties-files:

	portalstrings_de.properties
	portalstrings_en.properties
	portalstrings_fr.properties
	portalstrings_nl.properties
	
	(soaperrstrings.properies)  - error messages for SOAP interface

The locale in the file name defines the language of the translations
in the file. For the following languages are predefined standard
messages available:

  de = german
	en = english
	fr = french
	nl = netherlands

The languages have to match with the defined locales at the principal's
portal languages.


Extract of portalstrings_en.properties:
---------------------------------------
[...]
  #
  # Category DLC
  #
  Dlc_Checkin =Check-in/Check-out
  Dlc_Deleted =Deleted 
  Dlc_Drafts =Drafts
  Dlc_Favorites =Favorites
  Dlc_Inbox =Inbox 
  CSV_Label_DlcFile_FollowUp =Resubmission
[...]

The syntax of entries in these files are:
key =translation

Note: Do not do any modifications at these files because they will always
      be updated with new versions.



3. Adjusting or adding portalstrings-entries
============================================

If you want to add entries (because you will use them in PortalScript) or
you want to modify the translation of an entry, don't adjust the predefined
portalstrings-files, but do it in the following way:

Create a principal depending portalstrings-file matching the following
syntax:

   portalstrings_principal_de.properties

   e.g. portalstrings_peachit_de.properties

In this file you can add new entries for the principal peachit in the locale
"en". If you want to adjust a translation that is predefined then define the
messages in this file a second time with the adjusted value.

      e.g.  portalstrings_en.properties:
            ----------------------------
            CSV_Label_DlcFile_FollowUp =Resubmission

      
            portalstrings_peachit_en.properties:
            ------------------------------------
            CSV_Label_DlcFile_FollowUp =Follow-up


At startup the server reads the predefined standard messages and then looks
for the principal depending messages and adds / overwrites the predefined
values.



4. Adding a new locale
======================

If you want to add a new locale to the system (e.g. italy; it) then add
the locale "it" to the principal's portal languages and create a
portalstrings_it.properties file with all translations. Then you can use
it.


		 
5. Update from otrisPortal51 / ELC 3.51
========================================

If you have done adjustments in the "System Messages" in your old
environment you can easily create files from these messages, that
are stored in the database, even if you can't see them in the
PortalManager.

After update to otrisPortal60 / ELC 3.60 and import the backup, you have
to login into the PortalManager and execute the following maintenance
operation from the Administration-Menu:

	exportTranslationTable xx

Where xx stands for the locale of the language that you want to 
export:
	
	e.g. exportTranslationTable de
	
After the execution of the maintenance operation you can find a new
file in the server/locale directory. This new file is named like
portalstrings_principal_xx.properties, where principal is the name
of the principal that the System Messages are exported from and xx
is replaced by that locale that is exported.
	
	e.g. portalstrings_peachit_de.properties
	
The new file that is created contains only the System Messages that
differ from the existing global System Messages, so that files can
be much smaller than the global portalstrings-files.
