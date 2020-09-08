---
layout: single
permalink: /wiki/engineering/cpp
toc: true
---

## RAII and the Rule of Zero

### Classes that manager resources  
- Allocated memory (malloc/free, new/delete, new[]/delete[])
- POSIX file handles (open/close)
- C FILE handles(fopen/fclose)
- Mutex locks (pthread_mutex_lock/pthread_mutex_unlock)
- C++ threads (spawn/join)
- Ojective-C resource-counted objects(retrain/release)

> There is some explicit action that needs to be taken by the program in order to **free** the resource

### A naive implementation of vector  
```c++
class NaiveVector{
  int *ptr_;
  size_t size_;
public:
  NaiveVector(): ptr_(nullptr), size_t(0){}
  
  void push_back(int new_value) {
    int *newptr = new int [size_ + 1];
    std::copy(ptr_, ptr_ + size_, newptr);
    delete [] ptr_;
    ptr_ = newptr;
    ptr_[size_++] = newvalue;
  }
  
  ~NaiveVector() { delete [] ptr_; }  // Whenever you write a destructor, you probably 
                                      // need to write a copy constructor as well
                                      // 
                                      // The destructor is responsible for freeing resources to avoid
                                      // **leaks**, The copy constructor is responsible for duplicating
                                      // resources to avoid **double frees**
                                      //
                                      // This applies to memory, or any other resources you might be
                                      // managing.
                                      
                                      
};

```

```c++
// buggy below

{
  NaiveVector v;
  v.push_back(1);
  {
    NaiveVector w = v;
  }
  std::cout << v[0] << "\n";
}

```

### The Rule of Three  

If your class directly manages some kind of resource (such as a new'ed poniter), 
then you almost certainly need to hand-write three special member functions:  
  - A **destructor** to free the resource
  - A **copy constructor** to copy the resource
  - A **copy assignment operator** to free the left-hand resource and copy the right-hand one  

Use the copy-and-swap idiom to implement assignment

- [Every you wanted to know about move semantics](https://www.youtube.com/watch?v=vLinb2fgkHk&ab_channel=InsideBloomberg), [PPT](https://www.slideshare.net/ripplelabs/howard-hinnant-accu2014)

## Smart Pointers

- [CppCon 2019: Arthur O'Dwyer “Back to Basics: Smart Pointers”](https://www.youtube.com/watch?v=xGDLkt-jBJ4&list=PL5qoVlA-tv09ykIIPHP9N6vgJaFPnYWCa&index=3&ab_channel=CppCon)
- [You don’t need a stateful deleter in your **unique_ptr** (usually)](https://dev.krzaq.cc/post/you-dont-need-a-stateful-deleter-in-your-unique_ptr-usually/)

## Templats and Meta programmings
- [Substitution failure is not an error(**SFINAE**)](https://en.wikipedia.org/wiki/Substitution_failure_is_not_an_error)

## Threadings

- [C++ Core Guidelines: Be Aware of the Traps of Condition Variables](https://www.modernescpp.com/index.php/c-core-guidelines-be-aware-of-the-traps-of-condition-variables)
- [Memory model synchronization modes](http://gcc.gnu.org/wiki/Atomic/GCCMM/AtomicSync)
