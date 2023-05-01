Download Link: https://assignmentchef.com/product/solved-csc3022h-assignment3-huffman-tree-encoding
<br>



You are asked to write a compression algorithm to compress English language text files. You decide to use a Huffman encoder, which is a fairly quick and easy way of turning a sequence of ASCII characters into a compressed bit stream. For this tutorial, you need only implement the compression side, and you do not need to pack the output into a stream of bytes. You can simply use the ASCII characters 0 and 1 to represent the compressed bit representation. Of course, your compression factor will be 8x less as a consequence! So for extra credit, you can do some bit operations to pack the 0s and 1s into an array of bytes.

<h1>1           Constraints</h1>

The constraints for this tutorial are as follows:

<ol>

 <li>Your implementation must use at least a HuffmanTree class (which manages the tree and has methods to compress data, build a Huffman tree etc) and a HuffmanN-ode class, which represent the tree nodes in the underlying Huffman tree (which is a simple binary tree). These must be constructed and destructed Your constructor and destructor will need to be defined to use the specified smart pointers to manage memory see point 2) below.</li>

 <li>You must use std::shared <u>p</u>tr<em>&lt;</em>HuffmanNode<em>&gt;</em> to represent the left/right links for your tree nodes as well as the root of the tree in your HuffmanTree</li>

 <li>your destructors must *not* explicitly delete the tree node representation: this should happen automatically when the root node is set to “nullptr” in the Huff-manTree You should not call delete at all in the HuffmanNode destruc-tor. That is the benefit of using a managed pointer. You may, however, allocated your HuffmanTree instance on the heap using new, in which case you would call delete to free that up and the end of your program.</li>

 <li>You will need to implement: a default constructor, a move constructor, a copy constructor, a destructor, an assignment operator and a move assignment operator more details are provided 5. Yourprogramwillbedrivenfromthecommandline,andshouldbeinvokedasfollows:</li>

</ol>

huffencode <em>&lt;</em>inputFile<em>&gt;&lt;</em>output file<em>&gt;</em> w<u>h</u>ere huffencode is the name

of your application, inputFile is an input English (ASCII) text file and output file is the names of your output compressed “bitstream”.

Note: if you do not do any bit packing, this file may be larger than the input (!). To compute the actual packed file size in bytes (approximately) you can compute:

(Nbits/8) + (Nbits%8 ? 1 : 0) using integer arithmetic. This accommodates extra padding bits needed to deal with output bit stream sizes that are not a power of 2.

<ol start="6">

 <li>Prior to constructing the huffman encoding tree you have to count the number of occurrences of each letter in the To store these frequencies you can use a std::unordered map with keys of type character and values of integer type (the unordered map is very similar to Java’s HashMap). When you build the Huffman tree, you will need this information to decide how to insert nodes for each letter. The <a href="http://en.cppreference.com/w/cpp/container/unordered_map">relevant</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map">header</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map">file</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map">is</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map"><em>&lt;</em></a><a href="http://en.cppreference.com/w/cpp/container/unordered_map">unordered</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map">map</a><a href="http://en.cppreference.com/w/cpp/container/unordered_map"><em>&gt;</em></a><a href="http://en.cppreference.com/w/cpp/container/unordered_map">.</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map">M</a><a href="http://en.cppreference.com/w/cpp/container/unordered_map">ore</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map">information</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map">is</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map">available</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map">at</a> <a href="http://en.cppreference.com/w/cpp/container/unordered_map">http:// en.cppreference.com/w/cpp/container/unordered_map</a><a href="http://en.cppreference.com/w/cpp/container/unordered_map">.</a></li>

 <li>You should use a std::priority queue<em>&lt;</em>HuffmanNode<em>&gt;</em> data structure to assist you in the building the The relevant header file is <em>&lt;</em>queue<em>&gt;</em></li>

</ol>

A priority queue returns the next largest or smallest item, depending on how you have set it up. This is required to decide which tree nodes to aggregate as you build your tree. To ensure it works, you will need to provide a binary function predicate

bool compare(const HuffmanNode&amp; a, const HuffmanNode&amp; b)

{

If (a &lt; b) return true; // or &gt; if the algorithm requires that ordering else return false;

}

You will need to ensure the “<em>&lt;</em>” operation is evaluated sensibly for the type HuffmanNode , as required by the tree construction algorithm. This function (which can be a free function) will be used by the priority queue to decide how to order its HuffmanNode elements.

A priority queue supports a “push()” method (to insert elements), a “top()” method to copy the next element to be returned, and a “pop()” method to discard the next element to be returned. You can call “empty()” to see if the queue is empty or “size()” to see how many elements remain.

To create an instance of the priority queue you would have code like the following: std::priority queue<em>&lt;</em>HuffmanNode, compare<em>&gt;</em>myQueue; See <a href="http://www.cplusplus.com/reference/queue/priority_queue/pop/">http://www. </a><a href="http://www.cplusplus.com/reference/queue/priority_queue/pop/">cplusplus.com/reference/queue/priority_queue/pop/</a> for a simple usage with plain old ints.

<h1>2           Huffman compression algorithm</h1>

You can build a Huffman tree as follows:

<ol>

 <li>allocate a shared pointer<em>&lt;</em>HuffmanNode<em>&gt; </em>for each letter, which contains the letter and frequency;</li>

 <li>create a std::priority queue<em>&lt;</em>HuffmanNode, compare<em>&gt;</em>, making sure that the right ordering (i.e. return smallest item) has been set in the compare function; Then, repeat until the queue has only one node, which will be the tree root:</li>

 <li>choose the two nodes with the smallest letter frequency and create a new internal parent node which has these two nodes as its left and right The smaller nodes are insertedastheleftchild.Thesenodesshouldbepoppedoutofthequeue</li>

</ol>

-you do not need to examine them again; 4. thenewnode(aninternalnode)hasnoletterassociatedwithit,onlyafrequency.

This frequency is the sum of the frequencies of its two children.

<ol start="5">

 <li>insert this new node in to the priority queue</li>

</ol>

At the end of this process you will have built your Huffman tree. If you have done this correctly the smart pointers will be sharing resources correctly and will auto destroy when the HuffmanTree goes out of scope (or is deleted) at the end of your program.

<h1>3           Code table</h1>

Next, you need to build the “code table”: this is a mapping that gives a bit string code for each letter in your alphabet. The easiest way to do this (perhaps) is to recurse down the tree from the root building up a string as you go. When you hit a leaf node with a letter, you then output that letter with the corresponding bit string you have accumulated. When traversing the tree, add a “0” if branching left and add “1” if branching right. You can use an inorder walk and output when you get to a leaf node. It is up to you to decide how to store this code book. Ideally, finding symbol and its corresponding bit string should be fast.

<h1>4           Compression</h1>

To compress your ASCII text file, take each character, in turn, finding its bit string code and write this into the output buffer. This “buffer” can just be a std::string which you append characters/strings to. At the end, you can extract the c str() (a pointer to a byte array) and write this out to file as a block of bytes. You should write out your code table to a separate file named “output file”.hdr where output fi<u>l</u>e is passed using command line argument. The header should have a field count at the top and a field for every character (consisting of the character and its code representation).