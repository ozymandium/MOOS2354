CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

# --------------------------------------------------------------
# Indicate CMake 2.7 and above that we don't want to mix relative
#  and absolute paths in linker lib lists.
# Run "cmake --help-policy CMP0003" for more information.
# --------------------------------------------------------------
if(COMMAND cmake_policy)
      cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

PROJECT(uPlayBack)

#build a list of files to compile
SET(UPLAYBACK_SRCS 
uPlayBackMain.cpp
PlaybackWindow.cpp
PlaybackWindow.h
MOOSPlayBackV2.h
MOOSPlayBackV2.cpp)


#look for FLTK ourselves
FIND_PACKAGE( FLTK REQUIRED )


#fltk has OpenGL dependancy
FIND_PACKAGE( OpenGL REQUIRED )

IF(FLTK_FOUND)
    INCLUDE_DIRECTORIES(${FLTK_INCLUDE_DIR})
    
    LINK_DIRECTORIES(${CMAKE_LIBRARY_PATH})

    # Add executable
    #(note the WIN32 bit says under windows use /subsystem:windows not console
    #this prevents fltk app from producing an annoying console aswell
    #ADD_EXECUTABLE( uPlayBack WIN32 ${UPLAYBACK_SRCS})   
    # However, this does not work under windows in release mode
    if( APPLE )
        ADD_EXECUTABLE( uPlayBack MACOSX_BUNDLE ${UPLAYBACK_SRCS})   
    else( APPLE )
        ADD_EXECUTABLE( uPlayBack ${UPLAYBACK_SRCS})   
    endif( APPLE )
   
   LINK_DIRECTORIES(${CMAKE_LIBRARY_PATH})

    TARGET_LINK_LIBRARIES(uPlayBack ${MOOSLIBS} ${PLATFORM_LIBS}  fltkvw ${FLTK_LIBRARIES} ${OPENGL_LIBRARIES} )

    INSTALL(TARGETS uPlayBack
            RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
            BUNDLE DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
           )

ELSE(FLTK_FOUND)
   MESSAGE("uPB won't be built as FLTK was not found.")
ENDIF(FLTK_FOUND)
