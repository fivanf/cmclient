set(NEWGRF_SOURCE_FILES
        ${CMAKE_CURRENT_SOURCE_DIR}/4d470305-BaseCosts-5.0.tar
)

# Walk over all newgrf files, and generate a command to copy them
foreach(NEWGRF_SOURCE_FILE IN LISTS NEWGRF_SOURCE_FILES)
    get_filename_component(NEWRGF_SOURCE_FILE_NAME "${NEWGRF_SOURCE_FILE}" NAME)
    set(NEWGRF_BINARY_FILE "${CMAKE_BINARY_DIR}/newgrf/${NEWGRF_SOURCE_FILE_NAME}")

    add_custom_command_timestamp(OUTPUT ${NEWGRF_BINARY_FILE}
            COMMAND ${CMAKE_COMMAND} -E make_directory ${CMAKE_BINARY_DIR}/newgrf
            COMMAND ${CMAKE_COMMAND} -E copy
                    ${NEWGRF_SOURCE_FILE}
                    ${NEWGRF_BINARY_FILE}
            MAIN_DEPENDENCY ${NEWGRF_FILE}
            COMMENT "Copying ${NEWGRF_SOURCE_FILE_NAME} newgrf file"
    )

    list(APPEND NEWGRF_BINARY_FILES ${NEWGRF_BINARY_FILE})
endforeach()

add_custom_target_timestamp(newgrf_files
        DEPENDS
        ${NEWGRF_BINARY_FILES}
)

add_library(newgrf
        INTERFACE
)
add_dependencies(newgrf
        newgrf_files
)
add_library(openttd::newgrf ALIAS newgrf)
