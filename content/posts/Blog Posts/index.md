---
title: None
type: article 
layout: post 
categories: ['todo'] 
tags: ['untagged'] 
authors: ['damian'] 
draft: True
toc: false 
featured: false 
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
    width: -102
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
    position: 6
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
    position: 3
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
    position: 9
    skipPersist: false
    isHidden: false
    sortIndex: -1
    width: 237
    options:
      - { label: "git", value: "git", color: "hsl(104,100%,50%)"}
      - { label: "hugo", value: "hugo", color: "hsl(104,100%,50%)"}
      - { label: "staticsite", value: "staticsite", color: "hsl(104,100%,50%)"}
      - { label: "travel", value: "travel", color: "hsl(90, 95%, 90%)"}
      - { label: "story", value: "story", color: "hsl(266, 95%, 90%)"}
      - { label: "rest", value: "rest", color: "hsl(3, 95%, 90%)"}
      - { label: "workflow", value: "workflow", color: "hsl(294, 95%, 90%)"}
      - { label: "hobby", value: "hobby", color: "hsl(78, 95%, 90%)"}
      - { label: "study", value: "study", color: "hsl(75, 95%, 90%)"}
      - { label: "quotes", value: "quotes", color: "hsl(312, 95%, 90%)"}
      - { label: "golang", value: "golang", color: "hsl(169, 95%, 90%)"}
      - { label: "iac", value: "iac", color: "hsl(144, 95%, 90%)"}
      - { label: "terraform", value: "terraform", color: "hsl(174, 95%, 90%)"}
      - { label: "bicep", value: "bicep", color: "hsl(147, 95%, 90%)"}
      - { label: "Career", value: "Career", color: "hsl(181, 95%, 90%)"}
      - { label: "MVP", value: "MVP", color: "hsl(177, 95%, 90%)"}
      - { label: "Cisco Champion", value: "Cisco Champion", color: "hsl(111, 95%, 90%)"}
      - { label: "Azure", value: "Azure", color: "hsl(270, 95%, 90%)"}
      - { label: "Azure DevOps", value: "Azure DevOps", color: "hsl(355, 95%, 90%)"}
      - { label: "Git", value: "Git", color: "hsl(9, 95%, 90%)"}
      - { label: "Jekyll", value: "Jekyll", color: "hsl(154, 95%, 90%)"}
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
    position: 7
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
    position: 10
    skipPersist: false
    isHidden: false
    sortIndex: -1
    width: 164
    options:
      - { label: "Modularising Hugo", value: "Modularising Hugo", color: "hsl(238,100%,50%)"}
      - { label: "Sharing Knowledge", value: "Sharing Knowledge", color: "hsl(242, 95%, 90%)"}
      - { label: "Terraform", value: "Terraform", color: "hsl(105, 95%, 90%)"}
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  description:
    input: text
    accessorKey: description
    key: description
    id: description
    label: description
    position: 11
    skipPersist: false
    isHidden: false
    sortIndex: -1
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  layout:
    input: select
    accessorKey: layout
    key: layout
    id: layout
    label: layout
    position: 2
    skipPersist: false
    isHidden: false
    sortIndex: -1
    options:
      - { label: "post", value: "post", color: "hsl(52, 95%, 90%)"}
      - { label: "quote", value: "quote", color: "hsl(180, 95%, 90%)"}
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  authors:
    input: select
    accessorKey: authors
    key: authors
    id: authors
    label: authors
    position: 12
    skipPersist: false
    isHidden: false
    sortIndex: -1
    options:
      - { label: "[,miracle]", value: "[,miracle]", color: "hsl(130, 95%, 90%)"}
      - { label: "[,Edward Snowden]", value: "[,Edward Snowden]", color: "hsl(115, 95%, 90%)"}
      - { label: "[,damian]", value: "[,damian]", color: "hsl(356, 95%, 90%)"}
      - { label: "damian", value: "damian", color: "hsl(186, 95%, 90%)"}
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  featured:
    input: checkbox
    accessorKey: featured
    key: featured
    id: featured
    label: featured
    position: 8
    skipPersist: false
    isHidden: false
    sortIndex: -1
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  alias:
    input: text
    accessorKey: alias
    key: alias
    id: alias
    label: alias
    position: 13
    skipPersist: false
    isHidden: false
    sortIndex: -1
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  categories:
    input: text
    accessorKey: categories
    key: categories
    id: categories
    label: categories
    position: 14
    skipPersist: false
    isHidden: false
    sortIndex: -1
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  comments:
    input: checkbox
    accessorKey: comments
    key: comments
    id: comments
    label: comments
    position: 5
    skipPersist: false
    isHidden: false
    sortIndex: -1
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  image:
    input: text
    accessorKey: image
    key: image
    id: image
    label: image
    position: 15
    skipPersist: false
    isHidden: false
    sortIndex: -1
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  image_caption:
    input: text
    accessorKey: image_caption
    key: image_caption
    id: image_caption
    label: image_caption
    position: 16
    skipPersist: false
    isHidden: false
    sortIndex: -1
    config:
      enable_media_view: true
      link_alias_enabled: true
      media_width: 100
      media_height: 100
      isInline: false
      task_hide_completed: true
      footer_type: none
      persist_changes: false
  toc:
    input: checkbox
    accessorKey: toc
    key: toc
    id: toc
    label: toc
    position: 4
    skipPersist: false
    isHidden: false
    sortIndex: -1
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
  show_metadata_tags: false
filters:
  enabled: false
  conditions:
```