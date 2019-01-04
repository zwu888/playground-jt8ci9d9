# 
```C++ runnable

#include <string>
template <class T>
class Singleton
{
public:
  static T* get_instance(int v)
  {
    if (!instance_)
      instance_ = new T(v);
    return instance_;
  }
  
  static T* get_instance()
  {
    if (!instance_)
      instance_ = new T();
    return instance_;
  }
  
  static T* get_instance(std::string s)
  {
    if (!instance_)
      instance_ = new T(s);
    return instance_;
  } // Sounds familiar...
  static void destroy_instance()
  {
    delete instance_;
    instance_ = nullptr;
  }
  
private:
  static T* instance_;
};

template <class T> T* Singleton<T>::instance_ = nullptr;
// The classes that will be singletons:
class Single: public Singleton<Single>
{
public:
  Single(std::string s): s_(s) {}
private:
  std::string s_;
};

class Single1: public Singleton<Single1>
{
public:
  Single1() { s_ = "test"; }
  void display() { std::cout << s_ << std::endl; }
private:
  std::string s_;
};


class Map: public Singleton<Map>
{
public:
  Map(int scale): scale_(scale) {}
private:
  int scale_;
};

// How to create and destroy them:
int main()
{
  Single* s = Single::get_instance("Forever Alone");
  Map* m = Map::get_instance(42);
  // Use these singletons.
  Single1* s1 = Single1::get_instance();
  s1->display();
  
  Map::destroy_instance();
  Single::destroy_instance();
}
