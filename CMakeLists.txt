project(AppKit-gui)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
	cmake_policy(SET CMP0005 NEW)
endif(COMMAND cmake_policy)

add_definitions(-D__APPLE__ -D__MACH__)
add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-DHAVE_STDINT_H=1 -DHAVE_WCHAR_H=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__)
add_definitions(-D__ENVIRONMENT_MAC_OS_X_VERSION_MIN_REQUIRED__=1080)
add_definitions(-DOBJC_ZEROCOST_EXCEPTIONS=1 -DGNUSTEP -DPRIxPTR=\"lx\" -DPRIuPTR=\"lu\" -DPRIdPTR=\"ld\" -DPRIu64=\"ull\")
add_definitions(-U__linux__) # Because of broken ifdef in NSPage.m
add_definitions(-DGNUSTEP_BASE_LIBRARY -DHAVE_DISPATCH_DISPATCH_H -DHAVE_BACKTRACE -DHAVE_UNEXPECTED)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -D__DARWIN_UNIX03 -fPIC -w -fconstant-string-class=NSConstantString -fblocks -ggdb -O0")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -nostdlib -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")

if(BITS EQUAL 64)
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fobjc-runtime=gnustep")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fobjc-runtime=gnustep")
else(BITS EQUAL 64)
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fobjc-abi-version=1")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fobjc-abi-version=1")
endif(BITS EQUAL 64)

SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/darling")
SET(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE)
SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/Headers)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/Headers)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/Headers/Additions)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/Headers/Additions)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/Headers/Native)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/Headers/Native)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/Source)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../foundation)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/../../foundation)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../foundation/Headers)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/../../foundation/Headers)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../corefoundation/Headers)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/../../corefoundation/Headers)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../libobjc2)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../libdispatch)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../icu/icuSources/common)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../icu/icuSources/i18n)
include_directories(${DARLING_TOP_DIRECTORY}/platform-include)
include_directories(${DARLING_TOP_DIRECTORY}/kernel-include)
include_directories(${DARLING_TOP_DIRECTORY}/compiler-include)
include_directories(${DARLING_TOP_DIRECTORY}/src/libffi/include)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/stdtime/FreeBSD)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/gen)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/include)
include_directories(${DARLING_TOP_DIRECTORY}/src/duct/include)
include_directories(${DARLING_TOP_DIRECTORY}/src/libm/Source)
include_directories(${DARLING_TOP_DIRECTORY}/src/libmalloc/include/malloc)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../openssl/src/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../libxml2/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../libxslt/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../zlib)

set(OBJC2RUNTIME 1)
set(BASE_NATIVE_OBJC_EXCEPTIONS 1)
set(GS_SINT8 "signed char")
set(GS_UINT8 "unsigned char")
set(GS_SINT16 "signed short")
set(GS_UINT16 "unsigned short")
set(GS_SINT64 "signed long long")
set(GS_UINT64 "unsigned long long")
set(GS_SADDR "signed long")
set(GS_UADDR "unsigned long")
set(GS_FLT32 "float")
set(GS_FLT64 "double")
set(GS_HAVE_I64 1)
set(ac_cv_sizeof_short 2)
set(ac_cv_sizeof_int 4)
set(ac_cv_sizeof_long_long 8)
set(ac_cv_sizeof_float 4)
set(ac_cv_sizeof_double 8)
set(_GSC_S_SHT "_GSC_I16")
set(_GSC_S_INT "_GSC_I32")
set(_GSC_S_LNG "_GSC_I64")
set(_GSC_S_LNG_LNG "_GSC_I64")

if (BITS EQUAL 64)
	set(GS_SINT32 "signed int")
	set(GS_UINT32 "unsigned int")
	set(ac_cv_sizeof_long 8)
	set(ac_cv_sizeof_voidp 8)
	set(GS_SIZEOF_COND_T 48)
	set(GS_SIZEOF_MUTEX_T 64)
	set(GS_ALIGNOF_COND_T 8)
	set(GS_ALIGNOF_MUTEX_T 8)
	set(_GSC_S_LNG "_GSC_I64")
	set(SIZEOF_VOIDP 8)
	set(GS_HAVE_I128 1)
	set(GS_SINT128 "__int128_t")
	set(GS_UINT128 "__uint128_t")
else (BITS EQUAL 64)
	set(GS_SINT32 "signed long")
	set(GS_UINT32 "unsigned long")
	set(ac_cv_sizeof_long 4)
	set(ac_cv_sizeof_voidp 4)
	set(GS_SIZEOF_COND_T 28)
	set(GS_SIZEOF_MUTEX_T 44)
	set(GS_ALIGNOF_COND_T 4)
	set(GS_ALIGNOF_MUTEX_T 4)
	set(_GSC_S_LNG "_GSC_I32")
	set(SIZEOF_VOIDP 4)
	set(GS_SINT128 "struct { gsu8 a[16]; }")
	set(GS_UINT128 "struct { gsu8 a[16]; }")
	set(GS_HAVE_I128 0)
endif (BITS EQUAL 64)

set(HAVE_LIBXML 1)
set(HAVE_BLOCKS 1)
set(HAVE_GNUTLS 0)
set(HAVE_INET_PTON 1)
set(HAVE_INET_NTOP 1)
set(HAVE_LIBDISPATCH 1)
set(HAVE_ICU 1)
set(USE_ZLIB 1)
set(BUGGY_PTR_LIMITS 0)
set(NX_CONST_STRING_CLASS "NSConstantString")
if (BITS EQUAL 64)
	set(GS_NONFRAGILE 1)
else (BITS EQUAL 64)
	set(GS_NONFRAGILE 0)
endif (BITS EQUAL 64)
set(GS_MIXEDABI 0)
set(GS_FAKE_MAIN 0)
set(GS_PASS_ARGUMENTS 0)
set(HAVE_MDNS 0)
set(HAVE_AVAHI 0)
set(GS_HAVE_OBJC_ROOT_CLASS_ATTR 0)
set(GS_WORDS_BIGENDIAN 0)
set(INCLUDE_INTTYPES "#include <stdint.h>")

set(GNUSTEP_GUI_VERSION "0.25.0")
set(GNUSTEP_GUI_MAJOR_VERSION 0)
set(GNUSTEP_GUI_MINOR_VERSION 25)
set(GNUSTEP_GUI_SUBMINOR_VERSION 0)
set(GCC_VERSION "${CMAKE_CXX_COMPILER_VERSION}")

configure_file(
    Headers/Additions/GNUstepGUI/GSVersion.h.in
    Headers/Additions/GNUstepGUI/GSVersion.h
)
configure_file(config.h.in config.h)

set(appkit_gui_SRCS
	Source/Functions.m
	Source/NSActionCell.m
	Source/NSAffineTransform.m
	Source/NSAlert.m
	Source/NSAnimation.m
	Source/NSAnimationContext.m
	Source/NSApplication.m
	Source/NSArrayController.m
	Source/NSAttributedString.m
	Source/NSBezierPath.m
	Source/NSBitmapImageRep.m
	Source/NSBitmapImageRep+GIF.m
	Source/NSBitmapImageRep+ICNS.m
	Source/NSBitmapImageRep+JPEG.m
	Source/NSBitmapImageRep+PNG.m
	Source/NSBitmapImageRep+PNM.m
	Source/NSBox.m
	Source/NSBrowser.m
	Source/NSBrowserCell.m
	Source/NSBundleAdditions.m
	Source/NSButton.m
	Source/NSButtonCell.m
	Source/NSCachedImageRep.m
	Source/NSCell.m
	Source/NSClipView.m
	Source/NSCollectionView.m
	Source/NSCollectionViewItem.m
	Source/NSColor.m
	Source/NSColorList.m
	Source/NSColorPanel.m
	Source/NSColorPicker.m
	Source/NSColorSpace.m
	Source/NSColorWell.m
	Source/NSComboBox.m
	Source/NSComboBoxCell.m
	Source/NSControl.m
	Source/NSController.m
	Source/NSCursor.m
	Source/NSCustomImageRep.m
	Source/NSDataLink.m
	Source/NSDataLinkManager.m
	Source/NSDataLinkPanel.m
	Source/NSDocument.m
	Source/NSDocumentController.m
	Source/NSDrawer.m
	Source/NSEPSImageRep.m
	Source/NSEvent.m
	Source/NSFileWrapper.m
	Source/NSFont.m
	Source/NSFontDescriptor.m
	Source/NSFontManager.m
	Source/NSFontPanel.m
	Source/NSForm.m
	Source/NSFormCell.m
	Source/NSGradient.m
	Source/NSGlyphGenerator.m
	Source/NSGraphicsContext.m
	Source/NSHelpPanel.m
	Source/NSHelpManager.m
	Source/NSImage.m
	Source/NSImageCell.m
	Source/NSImageRep.m
	Source/NSImageView.m
	Source/NSInputManager.m
	Source/NSInputServer.m
	Source/NSInterfaceStyle.m
	Source/NSKeyValueBinding.m
	Source/NSLayoutManager.m
	Source/NSLevelIndicator.m
	Source/NSLevelIndicatorCell.m
	Source/NSMatrix.m
	Source/NSMenu.m
	Source/NSMenuView.m
	Source/NSMenuItem.m
	Source/NSMenuItemCell.m
	Source/NSMovie.m
	Source/NSMovieView.m
	Source/NSNib.m
	Source/NSNibConnector.m
	Source/NSNibBindingConnector.m
	Source/NSNibAXAttributeConnector.m
	Source/NSNibAXRelationshipConnector.m
	Source/NSObjectController.m
	Source/NSOpenGLContext.m
	Source/NSOpenGLPixelFormat.m
	Source/NSOpenGLView.m
	Source/NSOpenPanel.m
	Source/NSOutlineView.m
	Source/NSPageLayout.m
	Source/NSPanel.m
	Source/NSParagraphStyle.m
	Source/NSPasteboard.m
	Source/NSPopover.m
	Source/NSPopUpButton.m
	Source/NSPopUpButtonCell.m
	Source/NSPrinter.m
	Source/NSPrintInfo.m
	Source/NSPrintOperation.m
	Source/NSPrintPanel.m
	Source/NSProgressIndicator.m
	Source/NSResponder.m
	Source/NSRulerMarker.m
	Source/NSRulerView.m
	Source/NSSavePanel.m
	Source/NSSearchField.m
	Source/NSSearchFieldCell.m
	Source/NSSegmentedCell.m
	Source/NSSegmentedControl.m
	Source/NSScreen.m
	Source/NSScroller.m
	Source/NSScrollView.m
	Source/NSSecureTextField.m
	Source/NSSelection.m
	Source/NSShadow.m
	Source/NSSlider.m
	Source/NSSliderCell.m
	Source/NSSound.m
	Source/NSSpeechSynthesizer.m
	Source/NSSpellChecker.m
	Source/NSSplitView.m
	Source/NSStepper.m
	Source/NSStepperCell.m
	Source/NSStringDrawing.m
	Source/NSStatusBar.m
	Source/NSStatusItem.m
	Source/NSTabView.m
	Source/NSTabViewItem.m
	Source/NSTableColumn.m
	Source/NSTableHeaderView.m
	Source/NSTableHeaderCell.m
	Source/NSTableView.m
	Source/NSText.m
	Source/NSTextAttachment.m
	Source/NSTextBlock.m
	Source/NSTextContainer.m
	Source/NSTextField.m
	Source/NSTextFieldCell.m
	Source/NSTextList.m
	Source/NSTextStorage.m
	Source/NSTextTable.m
	Source/NSTextTableBlock.m
	Source/NSTextView.m
	Source/NSTextView_actions.m
	Source/NSToolbar.m
	Source/NSToolbarItem.m
	Source/NSToolbarItemGroup.m
	Source/NSTokenField.m
	Source/NSTokenFieldCell.m
	Source/NSTrackingArea.m
	Source/NSTreeController.m
	Source/NSTreeNode.m
	Source/NSUserDefaultsController.m
	Source/NSView.m
	Source/NSViewController.m
	Source/NSWindow.m
	Source/NSWindowController.m
	Source/NSWorkspace.m
	Source/GSAnimator.m
	Source/GSAutocompleteWindow.m
	Source/GSDisplayServer.m
	Source/GSHelpManagerPanel.m
	Source/GSInfoPanel.m
	Source/GSMemoryPanel.m
	Source/GSSlideView.m
	Source/GSTextStorage.m
	Source/GSTrackingRect.m
	Source/GSServicesManager.m
	Source/tiff.m
	Source/externs.m
	Source/linking.m
	Source/GSCharacterPanel.m
	Source/GSDragView.m
	Source/GSFontInfo.m
	Source/GSTable.m
	Source/GSHbox.m
	Source/GSVbox.m
	Source/GSKeyBindingAction.m
	Source/GSKeyBindingTable.m
	Source/GSTextFinder.m
	Source/GSLayoutManager.m
	Source/GSTypesetter.m
	Source/GSHorizontalTypesetter.m
	Source/GSGormLoading.m
	Source/GSIconManager.m
	Source/GSImageMagickImageRep.m
	Source/GSNibLoading.m
	Source/GSTheme.m
	Source/GSThemeDrawing.m
	Source/GSThemeInspector.m
	Source/GSThemeMenu.m
	Source/GSThemeOpenSavePanels.m
	Source/GSThemePanel.m
	Source/GSThemePrintPanels.m
	Source/GSThemeTools.m
	Source/GSThemeWindow.m
	Source/GSTitleView.m
	Source/GSToolTips.m
	Source/GSToolbarView.m
	Source/GSToolbarCustomizationPalette.m
	Source/GSStandardWindowDecorationView.m
	Source/GSWindowDecorationView.m
	Source/GSPrinting.m
	Source/GSPrintOperation.m
	Source/GSEPSPrintOperation.m
	Source/GSPDFPrintOperation.m
	Source/GSModelLoaderFactory.m
	Source/GSGormLoader.m
	Source/GSGModelLoader.m
	Source/GSNibLoader.m
	Source/GSXibElement.m
	Source/GSXibLoader.m
	Source/GSXibLoading.m
	Source/GSXibObjectContainer.m
	Source/GSXibParser.m
	Source/GSHelpAttachment.m
)

set_source_files_properties(${appkit_gui_SRCS} LANGUAGE C)

add_library(appkit-gui OBJECT ${appkit_gui_SRCS})

set(appkit_gui_images
	Images/GNUstep.tiff
	Images/GNUstepMenuImage.tiff
	Images/GSSearch.tiff
	Images/GSStop.tiff
	Images/LogoGNUstep.tiff
	Images/MagnifyGlass.tiff
	Images/NSAddTemplate.tiff
	Images/NSComboArrow.tiff
	Images/NSRatingLevelIndicator.tiff
	Images/NSRemoveTemplate.tiff
	Images/common_2DCheckMark.tiff
	Images/common_2DDash.tiff
	Images/common_3DArrowDown.tiff
	Images/common_3DArrowLeft.tiff
	Images/common_3DArrowRight.tiff
	Images/common_3DArrowRightH.tiff
	Images/common_3DArrowUp.tiff
	Images/common_ApplicationFolder.tiff
	Images/common_ArrowDown.tiff
	Images/common_ArrowDownH.tiff
	Images/common_ArrowLeft.tiff
	Images/common_ArrowLeftH.tiff
	Images/common_ArrowRight.tiff
	Images/common_ArrowRightH.tiff
	Images/common_ArrowUp.tiff
	Images/common_ArrowUpH.tiff
	Images/common_CenterTabStop.tiff
	Images/common_Close.tiff
	Images/common_CloseBroken.tiff
	Images/common_CloseBrokenH.tiff
	Images/common_CloseH.tiff
	Images/common_ClosedHandCursor.tiff
	Images/common_ColorSwatch.tiff
	Images/common_ComboBoxEllipsis.tiff
	Images/common_DecimalTabStop.tiff
	Images/common_Desktop.tiff
	Images/common_Diamond.tiff
	Images/common_Dimple.tiff
	Images/common_DimpleHoriz.tiff
	Images/common_DocsFolder.tiff
	Images/common_DownloadFolder.tiff
	Images/common_DownArrowSmall.tiff
	Images/common_Folder.tiff
	Images/common_GSFolder.tiff
	Images/common_HelpCursor.tiff
	Images/common_HelpLink.tiff
	Images/common_Home.tiff
	Images/common_HomeDirectory.tiff
	Images/common_ImageFolder.tiff
	Images/common_Info.tiff
	Images/common_LeftTabStop.tiff
	Images/common_LibraryFolder.tiff
	Images/common_MiniWindowTile.tiff
	Images/common_Miniaturize.tiff
	Images/common_MiniaturizeH.tiff
	Images/common_MusicFolder.tiff
	Images/common_Mount.tiff
	Images/common_Nibble.tiff
	Images/common_OpenHandCursor.tiff
	Images/common_Printer.tiff
	Images/common_RadioOff.tiff
	Images/common_RadioOn.tiff
	Images/common_Right.tiff
	Images/common_RightH.tiff
	Images/common_RightTabStop.tiff
	Images/common_Root_Apple.tiff
	Images/common_Root_PC.tiff
	Images/common_Root_SGI.tiff
	Images/common_Root_Sparc.tiff
	Images/common_Root_Sparc2.tiff
	Images/common_SliderHoriz.tiff
	Images/common_SliderVert.tiff
	Images/common_StepperDown.tiff
	Images/common_StepperDownHighlighted.tiff
	Images/common_StepperUp.tiff
	Images/common_StepperUpHighlighted.tiff
	Images/common_SwitchOff.tiff
	Images/common_SwitchOn.tiff
	Images/common_TabDownSelectedLeft.tiff
	Images/common_TabDownSelectedRight.tiff
	Images/common_TabDownSelectedToUnSelectedJunction.tiff
	Images/common_TabDownUnSelectedJunction.tiff
	Images/common_TabDownUnSelectedLeft.tiff
	Images/common_TabDownUnSelectedRight.tiff
	Images/common_TabDownUnSelectedToSelectedJunction.tiff
	Images/common_TabSelectedLeft.tiff
	Images/common_TabSelectedRight.tiff
	Images/common_TabSelectedToUnSelectedJunction.tiff
	Images/common_TabUnSelectedToSelectedJunction.tiff
	Images/common_TabUnSelectedJunction.tiff
	Images/common_TabUnSelectedLeft.tiff
	Images/common_TabUnSelectedRight.tiff
	Images/common_Tile.tiff
	Images/common_ToolbarClippedItemsMark.tiff
	Images/common_ToolbarCustomizeToolbarItem.tiff
	Images/common_ToolbarSeparatorItem.tiff
	Images/common_ToolbarShowColorsItem.tiff
	Images/common_ToolbarShowFontsItem.tiff
	Images/common_ToolbarSpaceItem.tiff
	Images/common_Unknown.tiff
	Images/common_UnknownApplication.tiff
	Images/common_UnknownTool.tiff
	Images/common_Unmount.tiff
	Images/common_UpAndDownArrowSmall.tiff
	Images/common_VideoFolder.tiff
	Images/common_copyCursor.tiff
	Images/common_linkCursor.tiff
	Images/common_noCursor.tiff
	Images/common_outlineCollapsed.tiff
	Images/common_outlineExpanded.tiff
	Images/common_outlineUnexpandable.tiff
	Images/common_ret.tiff
	Images/common_retH.tiff
	Images/common_ProgressIndeterminate_1.tiff
	Images/common_ProgressIndeterminate_2.tiff
	Images/common_ProgressIndeterminate_3.tiff
	Images/common_ProgressIndeterminate_4.tiff
	Images/common_ProgressIndeterminate_5.tiff
	Images/common_ProgressIndeterminate_6.tiff
	Images/common_ProgressSpinning_1.tiff
	Images/common_ProgressSpinning_2.tiff
	Images/common_ProgressSpinning_3.tiff
	Images/common_ProgressSpinning_4.tiff
	Images/common_ProgressSpinning_5.tiff
	Images/common_ProgressSpinning_6.tiff
	Images/common_ProgressSpinning_7.tiff
	Images/common_ProgressSpinning_8.tiff
	Images/page_landscape.tiff
	Images/page_portrait.tiff
	Images/common_MultipleSelection.tiff
	Images/common_RecyclerEmpty.tiff
	Images/common_RecyclerFull.tiff
	Images/NSColorPanel.tiff
	Images/NSFontPanel.tiff
	Images/NSCaution.tiff
	Images/NSInfo.tiff
	Images/NSPreferencesGeneral.tiff
	Images/NSAdvanced.tiff
	Images/NSUserAccounts.tiff

	Images/nsmapping.strings
)

install(FILES ${appkit_gui_images} DESTINATION libexec/darling/System/Library/Images)

set(appkit_gui_sounds
	Sounds/Basso.wav
	Sounds/Blow.wav
	Sounds/Bottle.wav
	Sounds/Frog.wav
	Sounds/Glass.wav
	Sounds/Hero.wav
	Sounds/Ping.wav
	Sounds/Pop.wav
	Sounds/Sosumi.wav
	Sounds/Submarine.wav
	Sounds/Tink.wav
)

install(FILES ${appkit_gui_sounds} DESTINATION libexec/darling/System/Library/Sounds)

install(FILES KeyBindings/DefaultKeyBindings.dict DESTINATION libexec/darling/System/Library/KeyBindings)
