OpenGL glPrimitiveRestartIndex有何作用？

第七版OGL红宝书2.6.4 重启图元没看懂，上网搜索找到这么一篇博，可惜还是生肉就试着翻译一下。
引用：http://www.cnblogs.com/shane/archive/2013/04/23/3037210.html

void glPrimitiveRestartIndex(GLuint index);
#opengl

Primitive restart functionality allows you to tell OpenGL that a particular index value means, not to source a vertex at that index, but to begin a new primitive of the same type with the next vertex. In essence, it is an alternative to glMultiDrawElements. This allows you to have an element buffer that contains multiple triangle strips or fans (or similar primitives where the start of a primitive has special behavior).

The way it works is with the function glPrimitiveRestartIndex. This function takes an index value. If this index is found in the index array, the system will start the primitive processing again as though a second rendering command had been issued. If you use a BaseVertex drawing function, this test is done before the base vertex is added to the restart. Using this feature also requires using glEnable to activate it, and the corresponding glDisable to turn it off.

Here is an example. Let's say you have an index array as follows:

{ 0 1 2 3 65535 2 3 4 5 }

If you render this as a triangle strip normally, you get 7 triangles. If you render it with glPrimitiveRestartIndex(65535) and the primitive restart enabled, then you will get 4 triangles:

{0 1 2}, {1 2 3}, {2 3 4}, {3 4 5}

Primitive restart works with any of the versions of these functions.

Warning: It is technically legal to use this with non-indexed rendering. You should not do this, as it will not give you a useful result.
