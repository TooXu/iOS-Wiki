`objc-runtime-new.h`

```objc
struct objc_object {
  isa_t isa;
}

struct objc_class : objc_object {
		// Class ISA;
		Class superClass;
		cache_t cache;
		class_data_bits_t bits;	
}

struct class_data_bits_t {
  class_rw_t* data() {
    return (class_rw_t *)(bits & FAST_DATA_MASK); 
  }
}

struct class_rw_t {
  const class_ro_t *ro;
  method_array_t methods;
  property_array_t properties;
  protocol_array_t protocols;
}

struct class_ro_t {
  const char * name;
  method_list_t * baseMethodList;
  protocol_list_t * baseProtocols;
  const ivar_list_t * ivars;
}

```





