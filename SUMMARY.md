<!--- @file
  Summary

  Copyright (c) 2008-2017, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

# Summary

* [EDK II Build Specification](README.md#edk-ii-build-specification)
* [Tables](TABLES.md#tables)
* [Figures](FIGURES.md#figures)
* [1 Introduction](1_introduction/README.md#1-introduction)
  * [1.1 Overview](1_introduction/README.md#11-overview)
  * [1.2 Target Audience](1_introduction/README.md#12-target-audience)
  * [1.3 Terms](1_introduction/13_terms.md#13-terms)
  * [1.4 Related Information](1_introduction/14_related_information.md#14-related-information)
  * [1.5 Conventions Used in this Document](1_introduction/15_conventions_used_in_this_document.md#15-conventions-used-in-this-document)
* [2 Design Discussion](2_design_discussion/README.md#2-design-discussion)
  * [2.1 Development Environments](2_design_discussion/21_development_environments.md#21-development-environments)
  * [2.2 UEFI/PI Firmware Images](2_design_discussion/22_uefipi_firmware_images.md#22-uefipi-firmware-images)
  * [2.3 Boot Sequence](2_design_discussion/23_boot_sequence.md#23-boot-sequence)
  * [2.4 Typical Flash Part Layout](2_design_discussion/24_typical_flash_part_layout.md#24-typical-flash-part-layout)
  * [2.5 Generic Build Process](2_design_discussion/25_generic_build_process.md#25-generic-build-process)
  * [2.6 Creating EFI Images](2_design_discussion/26_creating_efi_images.md#26-creating-efi-images)
  * [2.7 SKU Support](2_design_discussion/27_sku_support.md#27-sku-support)
* [3 UEFI and PI Image Specification](3_uefi_and_pi_image_specification.md#3-uefi-and-pi-image-specification)
* [4 EDK II Build Process Overview](4_edk_ii_build_process_overview/README.md#4-edk-ii-build-process-overview)
  * [4.1 EDK II Build System](4_edk_ii_build_process_overview/41_edk_ii_build_system.md#41-edk-ii-build-system)
  * [4.2 Build Process Overview](4_edk_ii_build_process_overview/42_build_process_overview.md#42-build-process-overview)
  * [4.3 Pre-Build Stage Overview](4_edk_ii_build_process_overview/43_pre-build_stage_overview.md#43-pre-build-stage-overview)
  * [4.4 Creating Binary EFI Images - $(MAKE) stage](4_edk_ii_build_process_overview/44_creating_binary_efi_images_-_make_stage.md#44-creating-binary-efi-images---make-stage)
  * [4.5 Post-Build Stage](4_edk_ii_build_process_overview/45_post-build_stage.md#45-post-build-stage)
  * [4.6 File Specifications](4_edk_ii_build_process_overview/46_file_specifications.md#46-file-specifications)
  * [4.7 File Extensions](4_edk_ii_build_process_overview/47_file_extensions.md#47-file-extensions)
* [5 Meta-Data File Specifications](5_meta-data_file_specifications/README.md#5-meta-data-file-specifications)
  * [5.1 Build Meta-Data File Formats](5_meta-data_file_specifications/51_build_meta-data_file_formats.md#51-build-meta-data-file-formats)
  * [5.2 tools_def.txt](5_meta-data_file_specifications/52_tools_def_txt.md#52-tools-deftxt)
  * [5.3 target.txt File](5_meta-data_file_specifications/53_target_txt_file.md#53-targettxt-file)
* [6 Quick Start](6_quick_start/README.md#6-quick-start)
  * [6.1 Environment Variables](6_quick_start/61_environment_variables.md#61-environment-variables)
  * [6.2 Build Scope](6_quick_start/62_build_scope.md#62-build-scope)
* [7 Build Environment](7_build_environment/README.md#7-build-environment)
  * [7.1 Build Scope](7_build_environment/71_build_scope.md#71-build-scope)
  * [7.2 Third Party Tools](7_build_environment/72_third_party_tools.md#72-third-party-tools)
  * [7.3 GUIDed Tools](7_build_environment/73_guided_tools.md#73-guided-tools)
* [8 Pre-Build AutoGen Stage](8_pre-build_autogen_stage/README.md#8-pre-build-autogen-stage)
  * [8.1 Overview](8_pre-build_autogen_stage/81_overview.md#81-overview)
  * [8.2 Auto-generation Process](8_pre-build_autogen_stage/82_auto-generation_process.md#82-auto-generation-process)
  * [8.3 Auto-generated code](8_pre-build_autogen_stage/83_auto-generated_code.md#83-auto-generated-code)
  * [8.4 Auto-generated PCD Database File](8_pre-build_autogen_stage/84_auto-generated_pcd_database_file.md#84-auto-generated-pcd-database-file)
  * [8.5 Auto-generated Makefiles](8_pre-build_autogen_stage/85_auto-generated_makefiles.md#85-auto-generated-makefiles)
  * [8.6 Binary Modules](8_pre-build_autogen_stage/86_binary_modules.md#86-binary-modules)
  * [8.7 Generated AsBuilt INF Files](8_pre-build_autogen_stage/87_generated_asbuilt_inf_files.md#87-generated-asbuilt-inf-files)
* [9 Build or $(MAKE) Stage](9_build_or_make_stage/README.md#9-build-or-make-stage)
  * [9.1 Overview](9_build_or_make_stage/91_overview.md#91-overview)
  * [9.2 Preprocess/Trim](9_build_or_make_stage/92_preprocesstrim.md#92-preprocesstrim)
  * [9.3 Compile/Assembly](9_build_or_make_stage/93_compileassembly.md#93-compileassembly)
  * [9.4 Static Link](9_build_or_make_stage/94_static_link.md#94-static-link)
  * [9.5 Dynamic Link](9_build_or_make_stage/95_dynamic_link.md#95-dynamic-link)
  * [9.6 Generate Module Images](9_build_or_make_stage/96_generate_module_images.md#96-generate-module-images)
  * [9.7 Generate Platform Images](9_build_or_make_stage/97_generate_platform_images.md#97-generate-platform-images)
* [10 Post-Build ImageGen Stage - FLASH](10_post-build_imagegen_stage_-_flash/README.md#10-post-build-imagegen-stage---flash)
  * [10.1 Overview of Flash Device Layout](10_post-build_imagegen_stage_-_flash/101_overview_of_flash_device_layout.md#101-overview-of-flash-device-layout)
  * [10.2 Parsing FDF Meta-Data File](10_post-build_imagegen_stage_-_flash/102_parsing_fdf_meta-data_file.md#102-parsing-fdf-meta-data-file)
  * [10.3 Build Intermediate Images](10_post-build_imagegen_stage_-_flash/103_build_intermediate_images.md#103-build-intermediate-images)
  * [10.4 Create the FV Image File(s)](10_post-build_imagegen_stage_-_flash/104_create_the_fv_image_files.md#104-create-the-fv-image-files)
  * [10.5 Create the FD image file(s)](10_post-build_imagegen_stage_-_flash/105_create_the_fd_image_files.md#105-create-the-fd-image-files)
* [11 Post-Build ImageGen Stage - Other](11_post-build_imagegen_stage_-_other/README.md#11-post-build-imagegen-stage---other)
  * [11.1 EFI PCI Option ROM Images](11_post-build_imagegen_stage_-_other/111_efi_pci_option_rom_images.md#111-efi-pci-option-rom-images)
  * [11.2 UEFI Applications](11_post-build_imagegen_stage_-_other/112_uefi_applications.md#112-uefi-applications)
  * [11.3 Capsules](11_post-build_imagegen_stage_-_other/113_capsules.md#113-capsules)
* [12 Build Changes and Customizations](12_build_changes_and_customizations/README.md#12-build-changes-and-customizations)
  * [12.1 Building for Debug](12_build_changes_and_customizations/121_building_for_debug.md#121-building-for-debug)
  * [12.2 Adding Custom Compression Tools](12_build_changes_and_customizations/122_adding_custom_compression_tools.md#122-adding-custom-compression-tools)
  * [12.3 Using Custom Build Tools](12_build_changes_and_customizations/123_using_custom_build_tools.md#123-using-custom-build-tools)
  * [12.4 Customizing Compilation for a Component](12_build_changes_and_customizations/124_customizing_compilation_for_a_component.md#124-customizing-compilation-for-a-component)
  * [12.5 Platform Specific ASL Tools](12_build_changes_and_customizations/125_platform_specific_asl_tools.md#125-platform-specific-asl-tools)
  * [12.6 Build Reproducibility](12_build_changes_and_customizations/126_build_reproducibility.md#126-build-reproducibility)
* [13 Build Reports](13_build_reports/README.md#13-build-reports)
  * [13.1 Build Report Generation Options](13_build_reports/131_build_report_generation_options.md#131-build-report-generation-options)
  * [13.2 Sample Launch Steps: NT32 platform](13_build_reports/132_sample_launch_steps_nt32_platform.md#132-sample-launch-steps-nt32-platform)
  * [13.3 Output](13_build_reports/133_output.md#133-output)
  * [13.4 Platform Summary](13_build_reports/134_platform_summary.md#134-platform-summary)
  * [13.5 Global PCD Section](13_build_reports/135_global_pcd_section.md#135-global-pcd-section)
  * [13.6 FD Section](13_build_reports/136_fd_section.md#136-fd-section)
  * [13.7 Module Section](13_build_reports/137_module_section.md#137-module-section)
  * [13.8 Execution Order Prediction Section](13_build_reports/138_execution_order_prediction_section.md#138-execution-order-prediction-section)
* [Appendix A Variables](appendix_a_variables.md#appendix-a-variables)
* [Appendix B tools_def.txt](appendix_b_toolsdef_txt.md#appendix-b-tools_deftxt)
* [Appendix C target.txt](appendix_c_targettxt.md#appendix-c-targettxt)
* [Appendix D build.exe command](appendix_d_buildexe_command/README.md#appendix-d-buildexe-command)
  * [D.1 Overview](appendix_d_buildexe_command/d1_overview.md#d1-overview)
  * [D.2 Makefile actions](appendix_d_buildexe_command/d2_makefile_actions.md#d2-makefile-actions)
  * [D.3 Build Targets and options](appendix_d_buildexe_command/d3_build_targets_and_options.md#d3-build-targets-and-options)
  * [D.4 Usage](appendix_d_buildexe_command/d4_usage.md#d4-usage)
* [Appendix E NT32 Platform Emulation](appendix_e_nt32_platform_emulation.md#appendix-e-nt32-platform-emulation)
* [Appendix F Firmware Volume INF](appendix_f_firmware_volume_inf/README.md#appendix-f-firmware-volume-inf)
  * [F.1 Firmware Volume INF Description](appendix_f_firmware_volume_inf/f1_firmware_volume_inf_description.md#f1-firmware-volume-inf-description)
  * [F.2 [Attributes] Section](appendix_f_firmware_volume_inf/f2_[attributes]_section.md#f2-attributes-section)
  * [F.3 [Files] Section](appendix_f_firmware_volume_inf/f3_[files]_section.md#f3-files-section)
  * [F.4 [Options] Section](appendix_f_firmware_volume_inf/f4_[options]_section.md#f4-options-section)
* [Appendix G VS2005 Team Suite Performance](appendix_g_vs2005_team_suite_performance/README.md#appendix-g-vs2005-team-suite-performance-profile)
  * [G.1 Step 1 - Create a new project](appendix_g_vs2005_team_suite_performance/g1_step_1_-_create_a_new_project.md#g1-step-1---create-a-new-project)
  * [G.2 Step 2 - Update the project](appendix_g_vs2005_team_suite_performance/g2_step_2_-_update_the_project.md#g2-step-2---update-the-project)
* [Appendix H Module Types](appendix_h_module_types.md#appendix-h-module-types)
* [Appendix I VPD Tool](appendix_i_vpd_tool/README.md#appendix-i-vpd-tool)
  * [I.1 Build System Output File Format](appendix_i_vpd_tool/i1_build_system_output_file_format.md#i1-build-system-output-file-format)
  * [I.2 VPD Tool Map File Format](appendix_i_vpd_tool/i2_vpd_tool_map_file_format.md#i2-vpd-tool-map-file-format)
* [Appendix J Makefiles](appendix_j_makefiles.md#appendix-j-makefiles)
  * [J.1 NMAKE Module Makefile Format](appendix_j_makefiles.md#j1-nmake-module-makefile-format)
* [Appendix K Third Party Tool Flags](appendix_k_third_party_tool_flags.md#appendix-k-third-party-tool-flags)
