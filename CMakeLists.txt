cmake_minimum_required (VERSION 2.6)


#include (${CMAKE_SOURCE_DIR}/cmake/CMakeLists_transition_package.cmake)


project (s9s-socat)

set(CMAKE_CURRENT_SOURCE_DIR ./)



set (CMAKE_C_IMPLICIT_LINK_LIBRARIES "c")
set (CPPFLAGS -I. )
set (DEFS -DHAVE_CONFIG_H )
#set (LIBS -lrt -lutil -Wl,-Bstatic -lssl -Wl,-Bstatic -lcrypto )
set (LIBS -lrt -lutil -Wl,-Bstatic -lssl -lcrypto -Wl,-Bdynamic -ldl)
set (CMAKE_C_FLAGS "-O -D_GNU_SOURCE -Wall -Wno-parentheses ${DEFS} ${CPPFLAGS}" )

set (UTLSRCS error.c dalan.c procan.c procan-cdefs.c hostan.c fdname.c sysutils.c utils.c nestlex.c vsnprintf_r.c snprinterr.c filan.c sycls.c sslcls.c)
file (GLOB SOURCES
    ${CMAKE_CURRENT_SOURCE_DIR}/xio*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/xio*.c
)
add_executable (s9s-socat socat.c ${SOURCES} ${UTLSRCS})

include_directories ("/usr/include")
target_link_libraries (s9s-socat ${LIBS})
#set_target_properties (s9s-socat PROPERTIES LINK_FLAGS -L/lib/)


# build a CPack driven installer package


set (CPACK_PACKAGE_NAME "s9s-socat")
set (CPACK_PACKAGE_VENDOR "Severalnines AB")
set (CPACK_PACKAGE_CONTACT "support@severalnines.com")
set (CPACK_PACKAGE_DESCRIPTION "A socat build that compiled for the same systems as ClusterControl Controller is built for.")
set (CPACK_PACKAGE_DESCRIPTION_SUMMARY "A socat build for ClusterControl Controller.")

set (CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/COPYING")
set (CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/COPYING.OpenSSL")
set (CPACK_PACKAGE_VERSION_MAJOR "1")
set (CPACK_PACKAGE_VERSION_MINOR "7")
set (CPACK_PACKAGE_VERSION_PATCH "3")
set (CPACK_PACKAGE_VERSION_BUILD "2")
set (CPACK_GENERATOR "RPM;DEB")
set (CPACK_RPM_PACKAGE_GROUP "Applications/System")
set (CPACK_DEBIAN_PACKAGE_SECTION "Utilities")
set (CPACK_RPM_PACKAGE_LICENSE "See /usr/local/share/s9s-socat/COPYING and /usr/local/share/s9s-socat/COPYING.OpenSSL")
include (CPack)

include (InstallRequiredSystemLibraries)
install (TARGETS s9s-socat
    RUNTIME DESTINATION local/bin
)
install (FILES ${CMAKE_CURRENT_SOURCE_DIR}/COPYING
  DESTINATION /usr/local/share/s9s-socat
)
install (FILES ${CMAKE_CURRENT_SOURCE_DIR}/COPYING.OpenSSL
  DESTINATION /usr/local/share/s9s-socat
)
install (FILES ${CMAKE_CURRENT_SOURCE_DIR}/doc/socat.1
  DESTINATION /usr/local/share/man/man1 RENAME s9s-socat.1
)


