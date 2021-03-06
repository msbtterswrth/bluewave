Development Cheatsheet
----------------------

### GitFlow

```bash
# Create branch
git checkout 8.x-5.x
git checkout -b [issue-number]-[issue-description]
git push -u origin [issue-number]-[issue-description]

# Create patch
git diff 8.x-5.x > [project_name]-[issue-description]-[issue-number]-00.patch

# Apply remote patch
curl https://www.drupal.org/files/issues/[project_name]-[issue-description]-[issue-number]-00.patch | git apply -

# Force apply patch
patch -p1  < [project_name]-[issue-description]-[issue-number]-00.patch

# Create interdiff
interdiff \
  [issue-number]-[old-comment-number].patch \
  [issue-number]-[new-comment-number].patch \
  > interdiff-[issue-number]-[old-comment-number]-[new-comment-number].txt

# Merge branch with all commits
git checkout 8.x-5.x
git merge [issue-number]-[issue-description]
git push

# Merge branch as a single new commit
git checkout 8.x-5.x
git merge --squash [issue-number]-[issue-description]
git commit -m 'Issue #[issue-number]: [issue-description]'
git push

# Delete branch
git branch -D [issue-number]-[issue-description]
git push origin :[issue-number]-[issue-description]
```

**Generate Drush Make and Composer Files**

```bash
drush webform-libraries-make > webform.libraries.make.yml
drush webform-libraries-composer > composer.json
```

**Manually Execute an Update Hook**

```bash
drush php-eval 'module_load_include('install', 'webform'); webform_update_8032()';
```

**Import and Export Configuration**

```bash
# Generate *.features.yml for the webform.module and sub-modules.
# These files will be ignored. @see .gitignore.
echo 'true' > webform.features.yml

echo 'true' > modules/webform_examples/webform_examples.features.yml
echo 'true' > modules/webform_example_element/webform_example_element.features.yml
echo 'true' > modules/webform_example_composite/webform_example_composite.features.yml
echo 'true' > modules/webform_example_element/webform_example_remote_post.features.yml

echo 'true' > modules/webform_templates/webform_templates.features.yml
echo 'true' > modules/webform_templates/webform_templates.features.yml

echo 'true' > modules/webform_image_select/webform_image_select.features.yml
echo 'true' > modules/webform_image_select/tests/modules/webform_image_select_test.features.yml

echo 'true' > modules/webform_node/webform_node.features.yml
echo 'true' > modules/webform_node/tests/modules/webform_node_test_multiple/webform_node_test_multiple.features.yml

echo 'true' > modules/webform_scheduled_email/tests/modules/webform_scheduled_email_test/webform_scheduled_email_test.features.yml

echo 'true' > modules/webform_demo/webform_demo_application_evaluation/webform_demo_application_evaluation.features.yml
echo 'true' > modules/webform_demo/webform_demo_event_registration/webform_demo_event_registration.features.yml

echo 'true' > tests/modules/webform_test/webform_test.features.yml
echo 'true' > tests/modules/webform_test_ajax/webform_test_ajax.features.yml
echo 'true' > tests/modules/webform_test_alter_hooks/webform_test_alter_hooks.features.yml
echo 'true' > tests/modules/webform_test_block_context/webform_test_block_context.features.yml
echo 'true' > tests/modules/webform_test_block_custom/webform_test_block_custom.features.yml
echo 'true' > tests/modules/webform_test_block_submission_limit/webform_test_block_submission_limit.features.yml
echo 'true' > tests/modules/webform_test_config_performance/webform_test_config_performance.features.yml
echo 'true' > tests/modules/webform_test_custom_properties/webform_test_custom_properties.features.yml
echo 'true' > tests/modules/webform_test_element/webform_test_element.features.yml
echo 'true' > tests/modules/webform_test_handler/webform_test_handler.features.yml
echo 'true' > tests/modules/webform_test_handler_remote_post/webform_test_handler_remote_post.features.yml
echo 'true' > tests/modules/webform_test_options/webform_test_options.features.yml
echo 'true' > tests/modules/webform_test_paragraphs/webform_test_paragraphs.features.yml
echo 'true' > tests/modules/webform_test_submissions/webform_test_submissions.features.yml
echo 'true' > tests/modules/webform_test_third_party_settings/webform_test_third_party_settings.features.yml
echo 'true' > tests/modules/webform_test_translation/webform_test_translation.features.yml
echo 'true' > tests/modules/webform_test_translation_lingotek/webform_test_translation_lingotek.features.yml
echo 'true' > tests/modules/webform_test_validate/webform_test_validate.features.yml
echo 'true' > tests/modules/webform_test_views/webform_test_views.features.yml
echo 'true' > tests/modules/webform_test_wizard_custom/webform_test_wizard_custom.features.yml

# Make sure all modules that are going to be exported are enabled
drush en -y webform\
  webform_demo_application_evaluation\
  webform_demo_event_registration\
  webform_examples\
  webform_examples\
  webform_example_element\
  webform_example_remote_post\
  webform_image_select\
  webform_node\
  webform_templates\
  webform_test\
  webform_test_element\
  webform_test_handler\
  webform_test_handler_remote_post\
  webform_test_options\
  webform_test_views\
  webform_test_submissions\
  webform_test_translation\
  webform_image_select_test\
  webform_node_test_multiple\
  webform_scheduled_email_test;

# Show the difference between the active config and the default config.
drush features-diff webform
drush features-diff webform_test

# Export webform configuration from your site.          
drush features-export -y webform
drush features-export -y webform_demo_application_evaluation
drush features-export -y webform_demo_event_registration
drush features-export -y webform_examples
drush features-export -y webform_example_element
drush features-export -y webform_example_composite
drush features-export -y webform_example_remote_post
drush features-export -y webform_node
drush features-export -y webform_image_select
drush features-export -y webform_templates
drush features-export -y webform_test
drush features-export -y webform_test_block_submission_limit
drush features-export -y webform_test_element
drush features-export -y webform_test_handler
drush features-export -y webform_test_handler_remote_post
drush features-export -y webform_test_options
drush features-export -y webform_test_views
drush features-export -y webform_test_submissions
drush features-export -y webform_test_translation
drush features-export -y webform_test_paragraphs
drush features-export -y webform_image_select_test
drush features-export -y webform_node_test_multiple
drush features-export -y webform_scheduled_email_test

# Revert all feature update to *.info.yml files.
git checkout -- *.info.yml

# Tidy webform configuration from your site.          
drush webform-tidy -y --dependencies webform
drush webform-tidy -y --dependencies webform_demo_application_evaluation
drush webform-tidy -y --dependencies webform_demo_event_registration
drush webform-tidy -y --dependencies webform_examples
drush webform-tidy -y --dependencies webform_example_element
drush webform-tidy -y --dependencies webform_example_composite
drush webform-tidy -y --dependencies webform_example_remote_post
drush webform-tidy -y --dependencies webform_image_select
drush webform-tidy -y --dependencies webform_node
drush webform-tidy -y --dependencies webform_templates
drush webform-tidy -y --dependencies webform_test
drush webform-tidy -y --dependencies webform_test_block_submission_limit
drush webform-tidy -y --dependencies webform_test_element
drush webform-tidy -y --dependencies webform_test_handler
drush webform-tidy -y --dependencies webform_test_handler_remote_post
drush webform-tidy -y --dependencies webform_test_options
drush webform-tidy -y --dependencies webform_test_views
drush webform-tidy -y --dependencies webform_test_submissions
drush webform-tidy -y --dependencies webform_test_translation
drush webform-tidy -y --dependencies webform_test_paragraphs
drush webform-tidy -y --dependencies webform_image_select_test
drush webform-tidy -y --dependencies webform_node_test_multiple
drush webform-tidy -y --dependencies webform_scheduled_email_test

# Re-import all webform configuration into your site.      
drush features-import -y webform
drush features-import -y webform_demo_application_evaluation
drush features-import -y webform_examples
drush features-import -y webform_example_element
drush features-import -y webform_example_composite
drush features-import -y webform_example_remote_post
drush features-import -y webform_node
drush features-import -y webform_image_select
drush features-import -y webform_templates
drush features-import -y webform_test
drush features-import -y webform_test_element
drush features-import -y webform_test_block_submission_limit
drush features-import -y webform_test_handler
drush features-import -y webform_test_handler_remote_post
drush features-import -y webform_test_options
drush features-import -y webform_test_views
drush features-import -y webform_test_submissions
drush features-import -y webform_test_translation
drush features-import -y webform_test_paragraphs
drush features-import -y webform_image_select_test
drush features-import -y webform_node_test_multiple
drush features-import -y webform_scheduled_email_test
```
