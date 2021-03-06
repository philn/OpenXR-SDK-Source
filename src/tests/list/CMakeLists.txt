# Copyright (c) 2017 The Khronos Group Inc.
#
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# Author:
#

add_executable(runtime_list
    list.cpp
)
add_dependencies(runtime_list
    generate_openxr_header
)
target_include_directories(runtime_list
    PRIVATE ${PROJECT_SOURCE_DIR}/src
    PRIVATE ${PROJECT_SOURCE_DIR}/src/common
    PRIVATE ${PROJECT_BINARY_DIR}/include
    PRIVATE ${PROJECT_SOURCE_DIR}/external/include
)
if(VulkanHeaders_FOUND)
    target_include_directories(runtime_list
        PRIVATE ${VulkanHeaders_INCLUDE_DIRS}
    )
endif()

if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    target_compile_options(runtime_list PRIVATE /Zc:wchar_t /Zc:forScope /W4 /WX)
    target_link_libraries(runtime_list openxr_loader)
endif()

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    target_compile_options(runtime_list PRIVATE -Wall)
    target_link_libraries(runtime_list openxr_loader pthread)
endif()

set_target_properties(runtime_list PROPERTIES FOLDER ${TESTS_FOLDER})
set_target_properties(runtime_list PROPERTIES OUTPUT_NAME openxr_runtime_list)


install(TARGETS runtime_list
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
if(NOT WIN32)
    install(FILES openxr_runtime_list.1 DESTINATION ${CMAKE_INSTALL_MANDIR}/man1/ COMPONENT ManPages)
endif()
