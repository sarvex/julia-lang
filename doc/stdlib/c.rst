.. currentmodule:: Base

*************
 C Interface
*************

.. function:: ccall((symbol, library) or fptr, RetType, (ArgType1, ...), ArgVar1, ...)

   Call function in C-exported shared library, specified by ``(function name, library)`` tuple, where each component is an ``AbstractString`` or ``Symbol``. Alternatively,
   ccall may be used to call a function pointer returned by dlsym, but note that this usage is generally discouraged to facilitate future static compilation.
   Note that the argument type tuple must be a literal tuple, and not a tuple-valued variable or expression.

.. function:: cglobal((symbol, library) or ptr [, Type=Void])

   Obtain a pointer to a global variable in a C-exported shared library, specified exactly as in ``ccall``.  Returns a ``Ptr{Type}``, defaulting to ``Ptr{Void}`` if no Type argument is supplied.  The values can be read or written by ``unsafe_load`` or ``unsafe_store!``, respectively.

.. function:: cfunction(fun::Function, RetType::Type, (ArgTypes...))

   Generate C-callable function pointer from Julia function. Type annotation of the return value in the
   callback function is a must for situations where Julia cannot infer the return type automatically.

   For example::

   	function foo()
   	    # body

   	    retval::Float64
   	end

   	bar = cfunction(foo, Float64, ())

.. function:: unsafe_load(p::Ptr{T},i::Integer)

   Load a value of type ``T`` from the address of the ith element (1-indexed)
   starting at ``p``. This is equivalent to the C expression ``p[i-1]``.

.. function:: unsafe_store!(p::Ptr{T},x,i::Integer)

   Store a value of type ``T`` to the address of the ith element (1-indexed)
   starting at ``p``. This is equivalent to the C expression ``p[i-1] = x``.

.. function:: unsafe_copy!(dest::Ptr{T}, src::Ptr{T}, N)

   Copy ``N`` elements from a source pointer to a destination, with no checking. The
   size of an element is determined by the type of the pointers.

.. function:: unsafe_copy!(dest::Array, do, src::Array, so, N)

   Copy ``N`` elements from a source array to a destination, starting at offset ``so``
   in the source and ``do`` in the destination (1-indexed).

.. function:: copy!(dest, src)

   Copy all elements from collection ``src`` to array ``dest``. Returns ``dest``.

.. function:: copy!(dest, do, src, so, N)

   Copy ``N`` elements from collection ``src`` starting at offset ``so``, to
   array ``dest`` starting at offset ``do``. Returns ``dest``.

.. function:: pointer(a[, index])

   Get the native address of an array or string element. Be careful to
   ensure that a julia reference to ``a`` exists as long as this
   pointer will be used.

.. function:: pointer(type, int)

   Convert an integer to a pointer of the specified element type.

.. function:: pointer_to_array(p, dims[, own])

   Wrap a native pointer as a Julia Array object. The pointer element type determines
   the array element type. ``own`` optionally specifies whether Julia should take
   ownership of the memory, calling ``free`` on the pointer when the array is no
   longer referenced.

.. function:: pointer_from_objref(obj)

   Get the memory address of a Julia object as a ``Ptr``. The existence of the resulting
   ``Ptr`` will not protect the object from garbage collection, so you must ensure
   that the object remains referenced for the whole time that the ``Ptr`` will be used.

.. function:: unsafe_pointer_to_objref(p::Ptr)

   Convert a ``Ptr`` to an object reference. Assumes the pointer refers to a
   valid heap-allocated Julia object. If this is not the case, undefined behavior
   results, hence this function is considered "unsafe" and should be used with care.

.. function:: disable_sigint(f::Function)

   Disable Ctrl-C handler during execution of a function, for calling
   external code that is not interrupt safe. Intended to be called using ``do``
   block syntax as follows::

    disable_sigint() do
        # interrupt-unsafe code
        ...
    end

.. function:: reenable_sigint(f::Function)

   Re-enable Ctrl-C handler during execution of a function. Temporarily
   reverses the effect of ``disable_sigint``.

.. function:: systemerror(sysfunc, iftrue)

   Raises a ``SystemError`` for ``errno`` with the descriptive string ``sysfunc`` if ``bool`` is true

.. data:: Cchar

   Equivalent to the native ``char`` c-type

.. data:: Cuchar

   Equivalent to the native ``unsigned char`` c-type (UInt8)

.. data:: Cshort

   Equivalent to the native ``signed short`` c-type (Int16)

.. data:: Cushort

   Equivalent to the native ``unsigned short`` c-type (UInt16)

.. data:: Cint

   Equivalent to the native ``signed int`` c-type (Int32)

.. data:: Cuint

   Equivalent to the native ``unsigned int`` c-type (UInt32)

.. data:: Clong

   Equivalent to the native ``signed long`` c-type

.. data:: Culong

   Equivalent to the native ``unsigned long`` c-type

.. data:: Clonglong

   Equivalent to the native ``signed long long`` c-type (Int64)

.. data:: Culonglong

   Equivalent to the native ``unsigned long long`` c-type (UInt64)

.. data:: Cintmax_t

   Equivalent to the native ``intmax_t`` c-type (Int64)

.. data:: Cuintmax_t

   Equivalent to the native ``uintmax_t`` c-type (UInt64)

.. data:: Csize_t

   Equivalent to the native ``size_t`` c-type (UInt)

.. data:: Cssize_t

   Equivalent to the native ``ssize_t`` c-type

.. data:: Cptrdiff_t

   Equivalent to the native ``ptrdiff_t`` c-type (Int)

.. data:: Coff_t

   Equivalent to the native ``off_t`` c-type

.. data:: Cwchar_t

   Equivalent to the native ``wchar_t`` c-type (Int32)

.. data:: Cfloat

   Equivalent to the native ``float`` c-type (Float32)

.. data:: Cdouble

   Equivalent to the native ``double`` c-type (Float64)
