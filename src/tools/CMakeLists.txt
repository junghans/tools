foreach(PROG votca_property)
  file(GLOB ${PROG}_SOURCES ${PROG}*.cc)
  add_executable(${PROG} ${${PROG}_SOURCES})
  target_link_libraries(${PROG} votca_tools)
  #not realy useful, re-enable whenever it is ready to replace csg_property
  install(TARGETS ${PROG} RUNTIME DESTINATION bin)

  if (BUILD_MANPAGES)
     add_custom_command(OUTPUT ${PROG}.man
       COMMAND ${CMAKE_CURRENT_BINARY_DIR}/${PROG} --man > ${PROG}.man
       COMMENT "Building ${PROG} manpage"
       DEPENDS ${PROG})
     add_custom_target(${PROG}_manpage DEPENDS ${PROG}.man)
     add_dependencies(manpages ${PROG}_manpage)
     install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROG}.man DESTINATION ${MAN}/man1 RENAME ${PROG}.1)
  endif (BUILD_MANPAGES)
endforeach(PROG)
