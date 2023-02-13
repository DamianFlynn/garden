---
title: None
type: article 
categories: ['todo'] 
tags: ['untagged'] 
draft: false 
toc: false 
comments: false 
---

```yaml:dbfolder
name: Blog Posts
description: Table of Blog Posts
columns:
  __file__:
    key: __file__
    id: __file__
    input: markdown
    label: File
    accessorKey: __file__
    isMetadata: true
    skipPersist: false
    isDragDisabled: false
    csvCandidate: true
    position: 1
    isHidden: false
    sortIndex: -1
    width: 104
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: true
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  date:
    input: calendar
    accessorKey: date
    key: date
    id: date
    label: date
    position: 3
    skipPersist: false
    isHidden: false
    sortIndex: -1
    width: 119
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  draft:
    input: checkbox
    accessorKey: draft
    key: draft
    id: draft
    label: draft
    position: 4
    skipPersist: false
    isHidden: false
    sortIndex: -1
    width: 136
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  tags:
    input: tags
    accessorKey: tags
    key: tags
    id: tags
    label: tags
    position: 5
    skipPersist: false
    isHidden: false
    sortIndex: -1
    width: 237
    options:
      - { label: "git", backgroundColor: "hsl(104,100%,50%)"}
      - { label: "hugo", backgroundColor: "hsl(104,100%,50%)"}
      - { label: "staticsite", backgroundColor: "hsl(104,100%,50%)"}
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  title:
    input: text
    accessorKey: title
    key: title
    id: title
    label: title
    position: 2
    skipPersist: false
    isHidden: false
    sortIndex: -1
    width: 269
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
      content_alignment: text-align-left
  series:
    input: select
    accessorKey: series
    key: series
    id: series
    label: series
    position: 100
    skipPersist: false
    isHidden: false
    sortIndex: -1
    width: 164
    options:
      - { label: "Modularising Hugo", backgroundColor: "hsl(238,100%,50%)"}
      - { label: "Sharing Knowledge", backgroundColor: "hsl(242, 95%, 90%)"}
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
config:
  remove_field_when_delete_column: false
  cell_size: normal
  sticky_first_column: false
  group_folder_column: 
  remove_empty_folders: false
  automatically_group_files: false
  hoist_files_with_empty_attributes: true
  show_metadata_created: false
  show_metadata_modified: false
  show_metadata_tasks: false
  show_metadata_inlinks: false
  show_metadata_outlinks: false
  source_data: current_folder
  source_form_result: 
  source_destination_path: /
  row_templates_folder: /
  current_row_template: 
  pagination_size: 10
  font_size: 16
  enable_js_formulas: false
  formula_folder_path: /
  inline_default: false
  inline_new_position: last_field
  date_format: yyyy-MM-dd
  datetime_format: "yyyy-MM-dd HH:mm:ss"
  metadata_date_format: "yyyy-MM-dd HH:mm:ss"
  enable_footer: false
  implementation: default
filters:
  enabled: false
  conditions:
```