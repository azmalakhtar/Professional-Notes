# 2. Structures
The first step in building a new type is often to organize the elements it needs into a data structure, a **struct**:
```cpp
struct Vector {
	int* elements;
	int size;
};

Vector* vector_init(int size) {
	Vector* vector = new Vector;
	vector->elements = new int[size];
	vector->size = size;

	return vector;
}
```

# 3. Classes
A tighter connection between representation and the operations is needed for a user-defined type to have all the properties of a real-type. We often want to keep the the representation inaccessible to all users so as to ease use, guarantee consistent use of data, and allow us to later improve the representation. 
```cpp
class Vector {
	public:
	Vector(int s): elements{new int[s]}, size{s} { }
	int& operator[](int index) { return elements[index];}
	int capacity() {return size;}
	private:
	int* elements;
	int size;
};
```

---
### References
- [C++ In-Depth Tour](C++%20In-Depth%20Tour.md)