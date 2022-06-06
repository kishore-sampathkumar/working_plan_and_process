## Requirement
Build a string vector using fixed size arrays and linked lists in C.

## Specification

### Data Structure
Functionally, a string vector can be described as containing the following:
- data structure to 'manage' user supplied strings (i.e., variable-sized
  array of characters allocated and managed by user) by storing their pointers
  as list elements
- A 'n_used' counter that tracks the current number of list elements that
  are stored in the vector
- A 'n_alloc' counter that tracks the total number of list elements that
  are allocated and hence can be stored in the vector
- A constant value 'n_min', that defines the lower limit for the number of
  list elements that can be stored in the vector
- A constant value 'n_max', that defines the upper limit for the number of
  list elements that can be stored in the vector

### Operations
Initialization is done by first calling 'initialize' operation. This results
in the vector being allocated and returned to the caller.

Users 'add', 'delete' or 'print' a string in the vector.

For 'add', user supplies the string to add to the vector. A unique integer
"index" value is returned on successfully adding the string to the vector.
This "index" value uniquely identifies the string that was added to the vector.

On failure to add, a value of -1 is returned. This value must be checked for
error conditions and handled accordingly.

A user can 'delete' or 'print' the string previously added.

For 'delete' and 'print', user must supply the unique 'index' value
corresponding to the string: this 'index' value was previously obtained
from an 'add' operation.

### Usage Model
Once an 'initialize' operation is done for the vector, a pointer to the vector
is returned. This pointer should then be supplied as the 1st argument for all
the operations on the vector.

The program does various 'add', 'delete' and 'print' operations as needed
and verifies that the vector has the correct data at at all times.

Once the program has completed all its operations, the program can simply
exit or return from the main program. The operating system automatically
handles the freeing of the vector and all resources allocated by it i.e.,
when the time comes to exit the program, there is no need to first 'delete'
all the 'add'ed strings, and then free the vector data structure initialized
at the beginning.

### Initialization and Boundary Conditions
The initial vector must be allocated a list of 128 pointers to hold at least
128 list elements. This is the lower limit for the size of the vector, and
shall be stored in n_min of the vector data structure.

The vector can grow up to 16384 list elements. This is the upper limit for
the size of vector, and shall be stored in n_max of the vector data structure.

Once initialized:
- the vector cannot shrink below the lower limit (n_min)
- The vector cannot grow beyond upper limit (n_max)

### Vector Management and Resizing
As part of 'initialize' operation,  the vector is allocated, n_min and n_max
are set to the prescribed values, allocation is automatically done to store
and manage n_min elements. Correspondingly, n_alloc will be set to n_min.
n_used will be set to 0.

Between the boundary conditions of n_min and n_max list elements, the vector
grows and shrinks as needed.

This happens automatically as part of addition and removal of strings to
the vector.

If an 'add' operation detects that there are some list elements allocated
(n_alloc) but not used (n_used), there is no need to resize the vector. The
unused element is allocated, n_used is incremented by 1, and the user is
returned the unique index corresponding to the list element just added.

If an 'add' operation detects that all the list elements allocated (n_alloc)
are aleady used (n_used), then, there is a need to resize the vector. This
is done by automatically allocating an additional 128 elements within the
boundary condition of a maximum of n_max elements. Hence, if n_alloc is
already equal to n_max (the upper limit), then, the 'add' operation will be
failed with a return value of -1.

For a 'delete' operation, the list element used to track the string
corresponding to the index supplied will now be marked as unused. This
results in decrementing n_used by 1.

If the 'delete' operation results in more than 128 elements being free,
then, a reallocation is automatically initiated to free 128 elements.

In summary, all reallocations are done in steps of 128 elements.

### Preconditions before Resizing
When the element for a ‘delete’ operation is not the last element, then, we
are deleting an element from the middle of the list. This creates a ‘hole’
in the list of elements. This makes it impossible to resize the list of
string pointers. To handle this, it is necessary to recognise this condition
and handle it i.e., on detecting that an element is being deleted from the
middle of the list, all the valid ‘used’ elements (indices) after this hole
should be moved back to the corresponding previous indices in order to
‘bubble-up’ this hole, and hence, close the hole. This ensures that ‘delete’
operations do not create holes. This results in the index value for each
string previously added to be changed (decremented by 1). The ‘caller’ is
expected to handle this in one of two ways:

- Once a string is added, make no assumption about which specific index value
  identifies this string. This is the case whenever we process all strings
  in the list, OR,

- Track the specific string (s1) added using the original index value
  returned on the first ‘add’ operation (let’s call this orig_index), and
  count the number of move operations done on delete via a specific counter
  maintained in the vector structure (n_move_on_delete). This number
  n_move_on_delete accurately specifies the difference between original index
  (orig_index) and the current index value for the given string s1 (let’s call
  this curr_index) i.e., curr_index = orig_index - n_move_on_delete.

  - Example 1: If orig_index = 10, n_move_on_delete = 0, curr_index = 10
    (i.e., 10 - 0)
  - Example 2: If orig_index = 10, n_move_on_delete = 4, curr_index = 6
    (i.e., 10 - 4)

However, the move operations done on delete may not affect a given specific
string (s1). This is because, the move happens from the ‘hole’ to the end of
the list. All list items _before_ the hole are not moved, and hence, are
unaffected. Hence, ’n_move_on_delete’ gives the absolute value i.e., the
total number of move operations done ever. For this to be useful to track a
given specific string (s1), the ’n_move_on_delete’ value should be checked
both _before_ and _after_ every ‘delete’ operation. If it changes, then, for
every specific string tracked whose orig_index is greater than the index used
for ‘delete’ operation, the orig_index for all those tracked strings should
immediately be decremented by 1. These are the strings that are _after_ the
hole and hence are affected by the move operation. All the specific strings
whose orig_index is lesser than the index used for ‘delete’ operation are
unaffected. These are the stings that are _before_ the hole and hence are not
affected by the move operation.

## Implementation Requirements
The below model should be followed while implementing vectors:

- The implementation of vector should be made available as a "library" called
  "libvector" that can be linked with user programs.

  This library should be a dynamically loadable shared object.

- User 'test' programs need to be written that will make function calls
  corresponding to the operations implemented in "libvector".

- The user test programs should be written such that they can support command
  line options.

- Any call to C standard library can fail (Example: malloc() can fail to
  allocate memory). The C code should recognize this, and handle the errors
  in a suitable manner.

- Every time there is an error, or every time there is a resize operation done,
  the same has to be 'logged' as a message and printed to the standard output.
  Note: If possible, in addition, the 'logging' should also be done in some
        user defined 'logfile' in a well defined location.
