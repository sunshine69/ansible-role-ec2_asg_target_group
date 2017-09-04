ec2_asg_target_group
--------------------

The `ec2_asg_target_group` role allows an ASG pointing at one target
group to be updated to point to another target group.

This is to allow an ASG pointing at the `new` target group to be
promoted to be in the `www` target group, without needing to know
anything else about that ASG.

## Variables

* `source_target_group` - the target group that the source ASG points to
* `target_target_groups` - the target groups that the ASG should be updated to point to

(I would think we name them like - existing_target_group and new_target_group -
that would be easier to understand)

## Examples

```
- hosts: "{{ application }}-{{ env }}"

  roles:
    # Ensure existing application is in the old target group
    - role: ec2_asg_target_group
      source_target_group: "{{ application }}-{{ env }}-www-tg"
      target_target_groups:
        - "{{ application }}-{{ env }}-old-tg"
        - "{{ application }}-{{ env }}-www-tg"

    # Add updated application to in the www target group
    - role: ec2_asg_target_group
      source_target_group: "{{ application }}-{{ env }}-new-tg"
      target_target_groups:
        - "{{ application }}-{{ env }}-new-tg"
        - "{{ application }}-{{ env }}-www-tg"

    # Remove existing application from the www target group
    - role: ec2_asg_target_group
      source_target_group: "{{ application }}-{{ env }}-old-tg"
      target_target_groups:
        - "{{ application }}-{{ env }}-old-tg"
```
