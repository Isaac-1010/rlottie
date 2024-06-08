cmake_policy(SET CMP0079 NEW)


# Initialize an empty list for dependencies
set(deps)

# Print whether EXT_FONT is enabled or not
if(EXT_FONT)
    message(STATUS "EXT_FONT is enabled")
else()
    message(STATUS "EXT_FONT is disabled")
endif()

if(EXT_FONT)
    find_package(Freetype REQUIRED)
    find_package(Fontconfig REQUIRED)
    find_package(HarfBuzz REQUIRED)

    list(APPEND deps ${FREETYPE_LIBRARIES} ${FONTCONFIG_LIBRARIES} ${HARFBUZZ_LIBRARIES})
endif()

# Add the source file
target_sources(rlottie
        PRIVATE
        "${CMAKE_CURRENT_LIST_DIR}/vtextshaper.cpp"
)

# Add include directories
target_include_directories(rlottie
        PRIVATE
        "${CMAKE_CURRENT_LIST_DIR}"
)

# Link the dependencies
if(EXT_FONT)
    target_link_libraries(rlottie PRIVATE ${deps})
endif()
