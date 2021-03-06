

INCLUDE_DIRECTORIES(
${BULLET_PHYSICS_SOURCE_DIR}/src 
${BULLET_PHYSICS_SOURCE_DIR}/Demos/SharedOpenCL
${BULLET_PHYSICS_SOURCE_DIR}/Demos/OpenGL
${NVIDIA_OPENCL_INCLUDES}
)




IF (CMAKE_CL_64)
	SET(CMAK_GLEW_LIBRARY
		${BULLET_PHYSICS_SOURCE_DIR}/Glut/glew64.lib		)
ELSE(CMAKE_CL_64)
	SET(CMAK_GLEW_LIBRARY		${BULLET_PHYSICS_SOURCE_DIR}/Glut/glew32.lib		)
ENDIF(CMAKE_CL_64)


IF (USE_GLUT)

	LINK_LIBRARIES(
		OpenGLSupport 
		BulletSoftBodySolvers_OpenCL_NVidia
		BulletMultiThreaded
		BulletSoftBody
		BulletDynamics  
		BulletCollision  
		LinearMath 
		${GLUT_glut_LIBRARY} 
		${OPENGL_gl_LIBRARY} 
		${OPENGL_glu_LIBRARY}
		${CMAK_GLEW_LIBRARY}
		${NVIDIA_OPENCL_LIBRARIES}
	)

	ADD_EXECUTABLE(AppOpenCLClothDemo_NVidia
		../cl_cloth_demo.cpp
		${BULLET_PHYSICS_SOURCE_DIR}/Demos/SharedOpenCL/btOclUtils.h
		${BULLET_PHYSICS_SOURCE_DIR}/Demos/SharedOpenCL/btOclCommon.h
		${BULLET_PHYSICS_SOURCE_DIR}/Demos/SharedOpenCL/btOclUtils.cpp
		${BULLET_PHYSICS_SOURCE_DIR}/Demos/SharedOpenCL/btOclCommon.cpp
		../gl_win.cpp
		../clstuff.cpp
		../bmpLoader.cpp
		../bmpLoader.h
		../clstuff.h
		../gl_win.h

	)
ELSE (USE_GLUT)
ENDIF (USE_GLUT)

IF(WIN32)
IF (CMAKE_CL_64)
	IF (NOT INTERNAL_CREATE_DISTRIBUTABLE_MSVC_PROJECTFILES)
		ADD_CUSTOM_COMMAND(	TARGET AppOpenCLClothDemo_NVidia		POST_BUILD
					COMMAND ${CMAKE_COMMAND} ARGS -E copy_if_different ${BULLET_PHYSICS_SOURCE_DIR}/glut64.dll ${CMAKE_CURRENT_BINARY_DIR}	
					)
		ADD_CUSTOM_COMMAND(	TARGET AppOpenCLClothDemo_NVidia	POST_BUILD
					COMMAND ${CMAKE_COMMAND} ARGS -E copy_if_different ${BULLET_PHYSICS_SOURCE_DIR}/GLEW64.DLL ${CMAKE_CURRENT_BINARY_DIR})
	ENDIF()
ELSE(CMAKE_CL_64)
	IF (NOT INTERNAL_CREATE_DISTRIBUTABLE_MSVC_PROJECTFILES)
		ADD_CUSTOM_COMMAND(	TARGET AppOpenCLClothDemo_NVidia	POST_BUILD
					COMMAND ${CMAKE_COMMAND} ARGS -E copy_if_different ${BULLET_PHYSICS_SOURCE_DIR}/GLUT32.DLL ${CMAKE_CURRENT_BINARY_DIR}
					)
		ADD_CUSTOM_COMMAND(	TARGET AppOpenCLClothDemo_NVidia	POST_BUILD
					COMMAND ${CMAKE_COMMAND} ARGS -E copy_if_different ${BULLET_PHYSICS_SOURCE_DIR}/GLEW32.DLL ${CMAKE_CURRENT_BINARY_DIR})

	ENDIF()
ENDIF(CMAKE_CL_64)
ENDIF(WIN32)
IF(NOT INTERNAL_CREATE_DISTRIBUTABLE_MSVC_PROJECTFILES)
	ADD_CUSTOM_COMMAND(	TARGET AppOpenCLClothDemo_NVidia	POST_BUILD
			COMMAND ${CMAKE_COMMAND} ARGS -E copy_if_different ${BULLET_PHYSICS_SOURCE_DIR}/Demos/OpenCLClothDemo/amdFlag.bmp ${CMAKE_CURRENT_BINARY_DIR}
			COMMAND ${CMAKE_COMMAND} ARGS -E copy_if_different ${BULLET_PHYSICS_SOURCE_DIR}/Demos/OpenCLClothDemo/atiFlag.bmp ${CMAKE_CURRENT_BINARY_DIR}
			)
ENDIF()
IF (UNIX)
  TARGET_LINK_LIBRARIES(AppOpenCLClothDemo_NVidia pthread)
ENDIF(UNIX)

IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
			SET_TARGET_PROPERTIES(AppOpenCLClothDemo_NVidia PROPERTIES  DEBUG_POSTFIX "_Debug")
			SET_TARGET_PROPERTIES(AppOpenCLClothDemo_NVidia PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
			SET_TARGET_PROPERTIES(AppOpenCLClothDemo_NVidia PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)