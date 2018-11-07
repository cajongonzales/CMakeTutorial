
# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
# state that we depend on Tutorial_BINARY_DIR but consumers don't, as the
# TutorialConfig.h include is an implementation detail

  # first we add the executable that generates the table
  add_executable(MakeTable MakeTable.cxx)

  # add the command to generate the source code
  add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/Table.h
    COMMAND MakeTable ${CMAKE_CURRENT_BINARY_DIR}/Table.h
    DEPENDS MakeTable
    )

  # add the main library
  add_library(MathFunctions
              mysqrt.cxx
              ${CMAKE_CURRENT_BINARY_DIR}/Table.h
              )


  target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          PUBLIC ${Tutorial_BINARY_DIR}
                 # add the binary tree directory to the search path for include files
                 ${CMAKE_CURRENT_BINARY_DIR}
          )





install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)
