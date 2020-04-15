# Copyright (c) 2016-2020 Minmin Gong
# 
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
# 
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
# 
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

cmake_minimum_required(VERSION 3.1)

project(UniversalDXSDK)

option(GENERATE_LIBRARYS "generate dxsdk librarys" ON)
option(SKIP_INSTALL_LIBRARYS "skip install librarys" OFF)

if (GENERATE_LIBRARYS)
    if (NOT EXISTS ${CMAKE_HOME_DIRECTORY}/Lib)
        execute_process(
            COMMAND "GenLibs.bat" 
            WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
            RESULT_VARIABLE result
        )

        if (NOT "_${result}_" STREQUAL "_0_")
            message(SEND_ERROR "generate librarys error...")
        endif()
    else()
        message(STATUS "universal dxsdk librarys have been generated...")
    endif()
endif()

set(UDXSDK_INSTALL_LIBDIR "Lib" CACHE PATH "universal dxsdk librarys install directorys" FORCE)
set(UDXSDK_INSTALL_INCDIR "Include" CACHE PATH "universal dxsdk include install directorys" FORCE)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/Include DESTINATION "." FILES_MATCHING PATTERN "*.*")

if (NOT SKIP_INSTALL_LIBRARYS)
    if (EXISTS ${CMAKE_HOME_DIRECTORY}/Lib)
        set(compileTargets x86 x64)

        foreach(var ${compileTargets})
            install(DIRECTORY ${PROJECT_SOURCE_DIR}/Lib/${var} DESTINATION "${UDXSDK_INSTALL_LIBDIR}" FILES_MATCHING PATTERN "*.lib" PATTERN "*.a")
        endforeach()        
    endif()
endif()

# add_library(UniversalDXSDK INTERFACE)
# target_include_directories(UniversalDXSDK
# 	INTERFACE
# 		${PROJECT_SOURCE_DIR}/Include
# )
