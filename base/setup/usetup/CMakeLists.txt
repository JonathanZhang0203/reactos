
add_definitions(${I18N_DEFS})

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/../lib
#    ${REACTOS_SOURCE_DIR}/base/setup/lib
    ${REACTOS_SOURCE_DIR}/sdk/lib/inflib
    ${REACTOS_SOURCE_DIR}/sdk/include/reactos/libs/zlib
    ${REACTOS_SOURCE_DIR}/sdk/include/reactos/drivers)

list(APPEND SOURCE
    bootsup.c
    cabinet.c
    chkdsk.c
    cmdcons.c
    console.c
    consup.c
    devinst.c
    drivesup.c
    filesup.c
    filequeue.c
    format.c
    fslist.c
    genlist.c
    inffile.c
    keytrans.c
    mui.c
    partlist.c
    progress.c
    registry.c
    settings.c
    usetup.c
    usetup.h)

add_executable(usetup ${SOURCE} usetup.rc)

if(USE_CLANG_CL)
    add_target_compile_flags(usetup "-Wno-invalid-source-encoding")
endif()

add_pch(usetup usetup.h SOURCE)
set_module_type(usetup nativecui)
target_link_libraries(usetup inflib setuplib zlib_solo ext2lib vfatlib)
add_importlibs(usetup ntdll)
add_cd_file(TARGET usetup DESTINATION reactos/system32 NO_CAB NAME_ON_CD smss.exe FOR bootcd regtest)
