# Binary Search Tree

- Accept [10_08_binary_search_tree](https://classroom.github.com/a/yZuyAczW)
- Get [basic_test.cpp](basic_test.cpp)

---

1. Implement `BST` class in [binary_search_tree.h](binary_search_tree.h)
2. Implement non-member functions for the `BST` class.
3. Test implementations by creating more complicated trees in `testB.cpp` file.

```
#ifndef BST_H
#define BST_H

#include <iostream>
#include <iomanip>

#include "binary_tree.h"

// NON-MEMBER FUNCTIONS for the BST<Item>:
template <class Item>
binary_tree_node<Item>* sortedArrayToBST(const Item* nums, int left, int right)
{
	binary_tree_node<Item>* root;
	root = new binary_tree_node<Item>(nums);
	return root;
}
template <class Item>
binary_tree_node<Item>* searchBST(binary_tree_node<Item>* root, int val)
{
	return root;
}
template <class Item>
binary_tree_node<Item>* insertIntoBST(binary_tree_node<Item>* root, int val)
{
	return root;
}
template <class Item>
binary_tree_node<Item>* deleteNode(binary_tree_node<Item>* root, int key)
{
	return root;
}
template <class Item>
binary_tree_node<Item>* mergeTrees(binary_tree_node<Item>* t1, binary_tree_node<Item>* t2)
{
	return t1;
}

template <class Item>
class BST
{
public:
	// TYPEDEFS
	typedef std::size_t size_type;
	typedef Item value_type;
	// CONSTRUCTORS and DESTRUCTOR
	BST() : root_ptr(nullptr) {}
	BST(const Item* nums, int size = -1) { root_ptr = NULL; } // nums is a sorted list
	BST(const BST<Item>& copy_me) { root_ptr = tree_copy(copy_me.root_ptr); }
	~BST() { clear_all(); }
	BST<Item>& operator = (const BST<Item>& rhs) {
		if (rhs == this) {
			return rhs;
		}
		else {
			tree_clear(root_ptr);
			root_ptr = tree_copy(rhs.root_ptr);
			return this;
		}
	}
	// MODIFICATION FUNCTIONS
	binary_tree_node<Item>* root() { return root_ptr; }
	binary_tree_node<Item>* search(const Item& target) {
		binary_tree_node<Item>* cursor = root_ptr;
		if (cursor == NULL || cursor->data() == target) { return cursor; }
		if (cursor->data() > target) { cursor = cursor->left(); return search(cursor, target); }
		else (cursor->data() < target) { cursor = cursor->right(); return search(cursor, target);}
	}
	void insert(const Item& val) {
		if (root_ptr == NULL) { root_ptr = new binary_tree_node<Item>(val); }

	}
	void erase(const Item& val){}
	void clear_all(){ tree_clear(root_ptr); }
	// CONST FUNCTIONS
	const binary_tree_node<Item>* root() const { return root_ptr; };
	bool empty() const { return root_ptr == nullptr; }
	// OVERLOAD OPERATOR FUNCTIONS
	template <class U>
	friend std::ostream& operator << (std::ostream& outs, const BST<U>& tree) {
		print(tree.root_ptr);
		return outs;
	}
	BST<Item> operator += (const BST<Item>& rhs) { return rhs; }
private:
	binary_tree_node<Item>* root_ptr; // Root pointer of binary search tree
};


// Implementation MEMBER FUNCTIONS

// TODO

// Implementation NON-MEMBER FUNCTIONS

// TODO

#endif // BST_H

```

