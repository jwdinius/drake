# IMPORTANT NOTE!!
# matlab has it's own boost libraries.  DO NOT let any mex file depend
# on the system boost (directly nor indirectly), or you're asking for trouble.

if(Matlab_FOUND)
  drake_add_mex(rbmMexFunctions rigidBodyTreeMexFunctions.cpp)
  drake_install_libraries(rbmMexFunctions)
  target_link_libraries(rbmMexFunctions drakeMexUtil drakeRBM)
  # These warnings are not worth fixing.
  target_compile_options(rbmMexFunctions PRIVATE ${CXX_FLAGS_NO_SIGN_COMPARE})

  macro(add_rbm_mex)
    drake_add_mex(${ARGV} ${ARGV}.cpp)
    target_link_libraries(${ARGV}
        rbmMexFunctions)
    # These warnings are not worth fixing.
    target_compile_options(${ARGV} PRIVATE ${CXX_FLAGS_NO_SIGN_COMPARE})
  endmacro()

  add_rbm_mex(constructModelmex)
  add_rbm_mex(collisionDetectmex)
  add_rbm_mex(smoothDistancePenaltymex)
  add_rbm_mex(collisionRaycastmex)
  add_rbm_mex(allCollisionsmex)
  add_rbm_mex(collidingPointsmex)
  add_rbm_mex(collidingPointsCheckOnlymex)
  add_rbm_mex(findKinematicPathmex)
  add_rbm_mex(geometricJacobianmex)
  add_rbm_mex(doKinematicsmex)
  add_rbm_mex(forwardKinmex)
  add_rbm_mex(forwardKinJacobianmex)
  add_rbm_mex(forwardKinPositionGradientmex)
  add_rbm_mex(centerOfMassmex)
  add_rbm_mex(centerOfMassJacobianmex)
  add_rbm_mex(centroidalMomentumMatrixmex)
  add_rbm_mex(massMatrixmex)
  add_rbm_mex(dynamicsBiasTermmex)
  add_rbm_mex(geometricJacobianDotTimesVmex)
  add_rbm_mex(centroidalMomentumMatrixDotTimesVmex)
  add_rbm_mex(centerOfMassJacobianDotTimesVmex)
  add_rbm_mex(forwardJacDotTimesVmex)
  add_rbm_mex(contactConstraintsmex)
  add_rbm_mex(surfaceTangentsmex)
  add_rbm_mex(jointLimitConstraintsmex)
  add_rbm_mex(positionConstraintsmex)
  add_rbm_mex(computeS1Trajmex)
  add_rbm_mex(createKinematicsCachemex)
  add_rbm_mex(createKinematicsCacheAutoDiffmex)
  add_rbm_mex(velocityToPositionDotMappingmex)
  add_rbm_mex(positionDotToVelocityMappingmex)
  add_rbm_mex(collisionDetectFromPointsmex)
  target_link_libraries(computeS1Trajmex drakeZMPUtil)

  if(gurobi_FOUND)
    drake_add_mex(solveLCPmex solveLCPmex.cpp)
    target_compile_definitions(solveLCPmex PRIVATE "PATHLCP_MEXFILE=\"${PROJECT_SOURCE_DIR}/thirdParty/misc/pathlcp/lcppath.${Matlab_MEX_EXTENSION}\"")
    target_link_libraries(solveLCPmex drakeRBM drakeMexUtil drakeQP)
  endif()

  macro(add_ikoptions_mex)
    drake_add_mex(${ARGV} ${ARGV}.cpp)
    target_link_libraries(${ARGV} drakeRBM drakeMexUtil drakeIKoptions)
    # These warnings are not worth fixing.
    target_compile_options(${ARGV} PRIVATE ${CXX_FLAGS_NO_SIGN_COMPARE})
  endmacro()
  add_ikoptions_mex(IKoptionsmex)

  macro(add_constraint_mex)
    drake_add_mex(${ARGV} ${ARGV}.cpp)
    target_link_libraries(${ARGV} drakeRBM drakeRigidBodyConstraint)
  endmacro()

  if(gurobi_FOUND)
    drake_add_mex(approximateIKmex approximateIKmex.cpp)
    target_link_libraries(approximateIKmex drakeIK drakeRBM drakeMexUtil drakeRigidBodyConstraint)
  endif()

  macro(add_ik_mex)
    drake_add_mex(${ARGV} ${ARGV}.cpp)
    target_link_libraries(${ARGV} drakeIK drakeRBM drakeMexUtil drakeRigidBodyConstraint)
    # These warnings are not worth fixing.
    target_compile_options(${ARGV} PRIVATE ${CXX_FLAGS_NO_SIGN_COMPARE})
  endmacro()

  if(snopt_FOUND)
    add_ik_mex(inverseKinmex)
    add_ik_mex(inverseKinPointwisemex)
    add_ik_mex(inverseKinTrajmex)
  endif()
endif()

add_subdirectory(collision)
add_subdirectory(constraint)
add_subdirectory(joints)
