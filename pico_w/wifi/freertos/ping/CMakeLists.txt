if (EXISTS ${PICO_LWIP_CONTRIB_PATH}/apps/ping/ping.c)
    add_executable(picow_freertos_ping_nosys
            picow_freertos_ping.c
            ${PICO_LWIP_CONTRIB_PATH}/apps/httpserver/httpserver-netconn.c
            )
    target_compile_definitions(picow_freertos_ping_nosys PRIVATE
            WIFI_SSID=\"${WIFI_SSID}\"
            WIFI_PASSWORD=\"${WIFI_PASSWORD}\"
            )
    target_include_directories(picow_freertos_ping_nosys PUBLIC
            ${CMAKE_CURRENT_LIST_DIR}
            ${CMAKE_CURRENT_LIST_DIR}/../.. # for our common lwipopts
            ${PICO_LWIP_CONTRIB_PATH}/apps/httpserver
            ${PICO_LWIP_CONTRIB_PATH}/ports/freertos/include
            ${PICO_LWIP_PATH}/src/include/lwip
            )
    target_link_libraries(picow_freertos_ping_nosys
            pico_cyw43_arch_lwip_threadsafe_background
            pico_stdlib
            pico_lwip_iperf
            FreeRTOS-Kernel-Heap4 # FreeRTOS kernel and dynamic heap
            )
    pico_add_extra_outputs(picow_freertos_ping_nosys)

    add_executable(picow_freertos_ping_sys
            picow_freertos_ping.c
            ${PICO_LWIP_CONTRIB_PATH}/apps/httpserver/httpserver-netconn.c
            )
    target_compile_definitions(picow_freertos_ping_sys PRIVATE
            WIFI_SSID=\"${WIFI_SSID}\"
            WIFI_PASSWORD=\"${WIFI_PASSWORD}\"
        #     NO_SYS=0            # don't want NO_SYS (generally this would be in your lwipopts.h) moved both to lwipopstss
        #     LWIP_SOCKET=1       # we need the socket API (generally this would be in your lwipopts.h)
        #     PING_USE_SOCKETS=1
            )
    target_include_directories(picow_freertos_ping_sys PUBLIC
            ${CMAKE_CURRENT_LIST_DIR}
            ${CMAKE_CURRENT_LIST_DIR}/../.. # for our common lwipopts
            ${PICO_LWIP_CONTRIB_PATH}/apps/httpserver
            ${PICO_LWIP_CONTRIB_PATH}/ports/freertos/include
            ${PICO_LWIP_PATH}/src/include/lwip
            )
    target_link_libraries(picow_freertos_ping_sys
            pico_cyw43_arch_lwip_sys_freertos
            pico_stdlib
            pico_lwip_iperf
            FreeRTOS-Kernel-Heap4 # FreeRTOS kernel and dynamic heap
            )
    pico_add_extra_outputs(picow_freertos_ping_sys)
endif()