include( polyhedron_demo_macros )
if(EIGEN3_FOUND)

  find_package(SCIP QUIET)
  if(NOT SCIP_FOUND)
    find_package(GLPK QUIET)
  endif()

  if(NOT SCIP_FOUND AND NOT GLPK_FOUND)
    message(STATUS "NOTICE: SCIP and GLPK were not found. Polygonal surface reconstruction will not be available.")
  endif()

  qt5_wrap_ui( surface_reconstructionUI_FILES Surface_reconstruction_plugin.ui)
  polyhedron_demo_plugin(surface_reconstruction_plugin Surface_reconstruction_plugin Surface_reconstruction_poisson_impl Surface_reconstruction_advancing_front_impl Surface_reconstruction_scale_space_impl Surface_reconstruction_polygonal_impl ${surface_reconstructionUI_FILES} KEYWORDS PointSetProcessing)
  target_link_libraries(surface_reconstruction_plugin PUBLIC scene_polygon_soup_item scene_surface_mesh_item scene_points_with_normal_item)
  CGAL_target_use_Eigen(surface_reconstruction_plugin)

  if (SCIP_FOUND)
    CGAL_target_use_SCIP(surface_reconstruction_plugin)
  elseif(GLPK_FOUND)
    CGAL_target_use_GLPK(surface_reconstruction_plugin)
  endif()

  qt5_wrap_ui( point_set_normal_estimationUI_FILES Point_set_normal_estimation_plugin.ui)
  polyhedron_demo_plugin(point_set_normal_estimation_plugin Point_set_normal_estimation_plugin ${point_set_normal_estimationUI_FILES} KEYWORDS PointSetProcessing Classification)
  target_link_libraries(point_set_normal_estimation_plugin PUBLIC scene_points_with_normal_item scene_callback_signaler)
  CGAL_target_use_Eigen(point_set_normal_estimation_plugin)

  qt5_wrap_ui( features_detection_pluginUI_FILES Features_detection_plugin.ui)
  polyhedron_demo_plugin(features_detection_plugin Features_detection_plugin ${features_detection_pluginUI_FILES} KEYWORDS PointSetProcessing)
  target_link_libraries(features_detection_plugin PUBLIC scene_points_with_normal_item)
  CGAL_target_use_Eigen(features_detection_plugin)

  polyhedron_demo_plugin(point_set_smoothing_plugin Point_set_smoothing_plugin KEYWORDS PointSetProcessing)
  target_link_libraries(point_set_smoothing_plugin PUBLIC scene_points_with_normal_item scene_callback_signaler)
  CGAL_target_use_Eigen(point_set_smoothing_plugin)

  polyhedron_demo_plugin(point_set_average_spacing_plugin Point_set_average_spacing_plugin KEYWORDS PointSetProcessing Classification)
  target_link_libraries(point_set_average_spacing_plugin PUBLIC scene_points_with_normal_item scene_callback_signaler)
  CGAL_target_use_Eigen(point_set_average_spacing_plugin)

  qt5_wrap_ui(point_set_shape_detectionUI_FILES Point_set_shape_detection_plugin.ui)
  polyhedron_demo_plugin(point_set_shape_detection_plugin Point_set_shape_detection_plugin ${point_set_shape_detectionUI_FILES} KEYWORDS PointSetProcessing Classification)
  target_link_libraries(point_set_shape_detection_plugin PUBLIC scene_surface_mesh_item scene_points_with_normal_item scene_polygon_soup_item scene_callback_signaler)
  CGAL_target_use_Eigen(point_set_shape_detection_plugin)

  find_package(OpenGR QUIET)
  find_package(libpointmatcher QUIET)

  if (OpenGR_FOUND OR libpointmatcher_FOUND)
    qt5_wrap_ui(register_point_setsUI_FILES Register_point_sets_plugin.ui)
    polyhedron_demo_plugin(register_point_sets_plugin Register_point_sets_plugin ${register_point_setsUI_FILES} KEYWORDS PointSetProcessing)
    target_link_libraries(register_point_sets_plugin PUBLIC scene_points_with_normal_item)
    CGAL_target_use_Eigen(register_point_sets_plugin)
    if (OpenGR_FOUND)
      CGAL_target_use_OpenGR(register_point_sets_plugin)
    endif()
    if (libpointmatcher_FOUND)
      CGAL_target_use_pointmatcher(register_point_sets_plugin)
    endif()
  else()
    message(STATUS "NOTICE: OpenGR and libpointmatcher were not found. Registrationp plugin will not be available.")
  endif()

else(EIGEN3_FOUND)
  message(STATUS "NOTICE: Eigen 3.1 (or greater) was not found. Surface reconstruction plugin will not be available.")
  message(STATUS "NOTICE: Eigen 3.1 (or greater) was not found. Normal estimation plugins will not be available.")
  message(STATUS "NOTICE: Eigen 3.1 (or greater) was not found. Smoothing plugin will not be available.")
  message(STATUS "NOTICE: Eigen 3.1 (or greater) was not found. Average spacing plugin will not be available.")
  message(STATUS "NOTICE: Eigen 3.1 (or greater) was not found. Feature detection plugin will not be available.")
endif()

  qt5_wrap_ui(point_set_bilateral_smoothingUI_FILES Point_set_bilateral_smoothing_plugin.ui)
  polyhedron_demo_plugin(point_set_bilateral_smoothing_plugin Point_set_bilateral_smoothing_plugin ${point_set_bilateral_smoothingUI_FILES} KEYWORDS PointSetProcessing)
  target_link_libraries(point_set_bilateral_smoothing_plugin PUBLIC scene_points_with_normal_item scene_callback_signaler)

  qt5_wrap_ui( ps_outliers_removal_UI_FILES Point_set_outliers_removal_plugin.ui)
  polyhedron_demo_plugin(point_set_outliers_removal_plugin Point_set_outliers_removal_plugin ${ps_outliers_removal_UI_FILES} KEYWORDS PointSetProcessing)
  target_link_libraries(point_set_outliers_removal_plugin PUBLIC scene_points_with_normal_item scene_callback_signaler)

  qt5_wrap_ui( point_set_selectionUI_FILES Point_set_selection_widget.ui)
  polyhedron_demo_plugin(point_set_selection_plugin Point_set_selection_plugin ${point_set_selectionUI_FILES} KEYWORDS PointSetProcessing Classification)
  target_link_libraries(point_set_selection_plugin PUBLIC scene_points_with_normal_item scene_polylines_item scene_edit_box_item)

  qt5_wrap_ui(point_set_simplificationUI_FILES Point_set_simplification_plugin.ui)
  polyhedron_demo_plugin(point_set_simplification_plugin Point_set_simplification_plugin ${point_set_simplificationUI_FILES} KEYWORDS PointSetProcessing)
  target_link_libraries(point_set_simplification_plugin PUBLIC scene_points_with_normal_item scene_callback_signaler)

  qt5_wrap_ui(point_set_upsamplingUI_FILES Point_set_upsampling_plugin.ui)
  polyhedron_demo_plugin(point_set_upsampling_plugin Point_set_upsampling_plugin ${point_set_upsamplingUI_FILES} KEYWORDS PointSetProcessing)
  target_link_libraries(point_set_upsampling_plugin PUBLIC scene_points_with_normal_item)

  qt5_wrap_ui(point_set_wlopFILES Point_set_wlop_plugin.ui)
  polyhedron_demo_plugin(point_set_wlop_plugin Point_set_wlop_plugin ${point_set_wlopFILES} KEYWORDS PointSetProcessing)
  target_link_libraries(point_set_wlop_plugin PUBLIC scene_points_with_normal_item scene_callback_signaler)

  polyhedron_demo_plugin(point_set_clustering_plugin Point_set_clustering_plugin KEYWORDS PointSetProcessing)
  target_link_libraries(point_set_clustering_plugin PUBLIC scene_points_with_normal_item scene_callback_signaler)

  polyhedron_demo_plugin(merge_point_sets_plugin Merge_point_sets_plugin KEYWORDS PointSetProcessing Classification)
  target_link_libraries(merge_point_sets_plugin PUBLIC scene_points_with_normal_item)

  polyhedron_demo_plugin(point_set_interference_plugin Point_set_interference_plugin KEYWORDS PointSetProcessing)
  target_link_libraries(point_set_interference_plugin PUBLIC scene_points_with_normal_item)

  qt5_wrap_ui( alpha_shapeUI_FILES Alpha_shape_widget.ui )
  polyhedron_demo_plugin(alpha_shape_plugin Alpha_shape_plugin ${alpha_shapeUI_FILES} KEYWORDS PointSetProcessing)
  target_link_libraries(alpha_shape_plugin PUBLIC scene_points_with_normal_item scene_c3t3_item)

  qt5_wrap_ui( distanceUI_FILES Point_set_to_mesh_distance_widget.ui )
  polyhedron_demo_plugin(point_set_to_mesh_distance_plugin Point_set_to_mesh_distance_plugin ${distanceUI_FILES} KEYWORDS PointSetProcessing)
  target_link_libraries(point_set_to_mesh_distance_plugin PUBLIC scene_points_with_normal_item scene_surface_mesh_item scene_color_ramp)

  if(TBB_FOUND)
    foreach(plugin
        surface_reconstruction_plugin
        point_set_normal_estimation_plugin
        features_detection_plugin
        point_set_smoothing_plugin
        point_set_average_spacing_plugin
        point_set_shape_detection_plugin
        point_set_bilateral_smoothing_plugin
        point_set_outliers_removal_plugin
        point_set_selection_plugin
        point_set_simplification_plugin
        point_set_upsampling_plugin
        point_set_wlop_plugin
        merge_point_sets_plugin
        point_set_interference_plugin
        alpha_shape_plugin
        point_set_to_mesh_distance_plugin)
      if(TARGET ${plugin})
        CGAL_target_use_TBB(${plugin})
      endif()
    endforeach()
  endif()
