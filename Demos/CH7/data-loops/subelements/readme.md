## The subelements Lookup Plugin (based on sub.yml )
* In Ansible, the subelements lookup plugin is used to iterate over a list of dictionaries while also iterating over a list defined within each of those dictionaries. 

* The general syntax for using subelements is as follows:
     ```
      loop: "{{ list | subelements('sublist_key') }}"
     ```

* In your playbook, list is the users variable, which is a list of dictionaries, and 'sublist_key' is 'groups', which is the key used to access the sublist in each dictionary.

### Understanding item.0 and item.1
- When you use subelements in a loop, it provides two values for each iteration:

* item.0: This gives you the current dictionary from the list being iterated over (in this case, a dictionary representing a user).

* item.1: This gives you the current dictionary from the sublist being iterated over (in this case, a dictionary representing a group).

### The Loop Iteration
* Given the data structure:
  ```
  users:
  - name: alice
    groups:
      - name: admin
        permissions:
          - read
          - write
      - name: users
        permissions:
          - read
  - name: bob
    groups:
      - name: users
        permissions:
          - read
  ```

  * The loop with subelements will perform iterations as follows:

- First Iteration:
    * item.0: The first dictionary in the users list (representing Alice).
    * item.1: The first dictionary in Alice's groups list (representing the admin group).
    * Debug Message: "User Alice is in the admin group with the following permissions: read, write"
- Second Iteration:
    * item.0: The first dictionary in the users list (still representing Alice).
    * item.1: The second dictionary in Alice's groups list (representing the users group).
    * Debug Message: "User Alice is in the users group with the following permissions: read"
- Third Iteration:
    * item.0: The second dictionary in the users list (representing Bob).
    * item.1: The first (and only) dictionary in Bob's groups list (also representing the users group).
    * Debug Message: "User Bob is in the users group with the following permissions: read"

### The join Filter
* Inside the debug message, we use the join filter to convert the list of permissions into a single string, where each permission is separated by a comma and a space. This is done with the following code:
   ```
   {{ item.1.permissions | join(', ') }}
   ```
  * For example, in the first iteration, item.1.permissions is the list ['read', 'write'], so the join filter converts this to the string 'read, write'.

### Summary
* Through the use of loop with subelements, your playbook is able to iterate over nested data structures and access deeply nested information to produce detailed debug messages about each user, their groups, and the permissions of those groups. This makes subelements a powerful tool for working with complex data structures in Ansible.

---
 *Steven* | *smahalin@redhat.com*

