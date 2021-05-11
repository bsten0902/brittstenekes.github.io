---
title: "How Can I Make My Code Easy To Read?"
summary: Practice writing documentation and unit tests.
date: 2021-04-21

weight: 2
tags: [ "Queuing", "Messy Data"]
author: "Brittany Stenekes"
showToc: false
ShowReadingTime: false
ShowBreadCrumbs: false
ShowPostNavLinks: true
hidemeta: true
---

Prior to taking the "Big Data Technologies" (ORIE 5270) course at Cornell University,
I had learned to code in several languages: python, VBA, java, swift, AMPL, ... etc.  

*Learning to code was the easy part.  
Learning to write code that is concise and easy for the naked eye to understand--that's another story.*

#### Here is my first shot at writing proper documentation and unit testing for the Node and LinkedList classes:


	#I referenced: https://realpython.com/linked-lists-python/

	class Node:
	    """
	    A class to represent a node
	    
	    ...
	    
	    Attributes
	    ----------
	    data: any object
	    
	    	The data contained by the node
	    	
	    next: Node
	    
	    	The reference to the next node in linked list
	    """
	    
	    def __init__(self, data):
	        """
	        Creates node object and initializes the node contents 
	        and the next node reference.
	        Parameters
	        ----------
	        next: ListNode
	        
	          Reference to the next node.
	          
	        data: Any
	        
	          The data contained in the node.
	        """
	        self.data = data
	        self.next = None
	
	    def get_Data(self):
	        """
	        Views a node's data.
	        Returns
	        ----------
	        data: Any 
	        
	          The data contained in the node.
	        """
	        return self.data
	
	class LinkedList:
	    def __init__(self, nodes=None):
	        """
	        Creates LinkedList and initializes the head node.
	        Also enables a linkedlist to be created by passing 
	        a list of data to the 'nodes' argument.
	        Parameters
	        -------------
	        nodes: List of Any
	        
	          List to create the linked list from, ordered in the desired
	          order of linked list nodes.
	        """
	        self.head = None
	        if nodes is not None and nodes!=[]:
	            node = Node(data=nodes.pop(0))
	            self.head = node
	            for elem in nodes:
	                node.next = Node(data=elem)
	                node = node.next
	                
	    def __iter__(self):
	        """
	        Makes the list iterable. This way, each 
	        consecutive node can in the list can be iterated 
	        over.
	        """
	        node = self.head
	        while node is not None:
	            yield node
	            node = node.next
	
	    def add_first(self, node):
	        """
	        Inserts a node at the beginning of the list.
	        Parameters
	        ----------
	        node: Node
	        
	          The node to add.
	        """
	        node.next = self.head
	        self.head = node
	
	    def add_last(self, node):
	        """
	        Inserts a node at the end of the list.
	        Parameters
	        ----------
	        node: Node
	        
	          The node to add.
	        """
	
	        if self.head is None:
	            self.head = node
	            return
	        for current_node in self:
	            pass
	        current_node.next = node
	        
	    def add_after(self, target_node_data, new_node):
	        """
	        Inserts a node AFTER a particular node.
	        Starting from the head, traverses the LinkedList until it reaches 
	        a node that contains the specified data. Then, inserts the new node
	        by updating next node references.
	        Parameters
	        ----------
	         target_node_data: Any
	         
	          The data contained in the node that should be deleted.
	          
	        new_node: Any
	        
	          The node to add.
	        """
	
	        if self.head is None:
	            raise Exception("List is empty")
	
	        for node in self:
	            if node.data == target_node_data:
	                new_node.next = node.next
	                node.next = new_node
	                return
	
	        raise Exception("Node with data '%s' not found" % target_node_data)
	
	    def remove_node(self, target_node_data):
	        """
	        Deletes the node that contains the specified data.
	        Starting from the head, traverses the LinkedList until it reaches 
	        a node that contains the specified data. Then, removes this node
	        from the list by reassigning next node references.
	        Parameters
	        ----------
	        target_node_data: Any
	        
	          The data contained in the node that should be deleted.
	        """
	
	        if self.head is None:
	            raise Exception("List is empty")
	
	        if self.head.data == target_node_data:
	            self.head = self.head.next
	            return
	
	        previous_node = self.head
	        for node in self:
	            if node.data == target_node_data:
	                previous_node.next = node.next
	                return
	            previous_node = node
	
	        raise Exception("Node with data '%s' not found" % target_node_data)
	
	    def length(self):
	        """
	        Counts the number of nodes.
	        
	        Returns
	        ----------
	        l: Int
	        
	          Number of nodes.
	        """
	        if self.head is None:
	          return 0
	        
	        current = self.head
	        l = 0
	        while current is not None:
	          l+=1
	          current = current.next
	        return l



And the unit testing: 

	import unittest
	from LinkedList import Node
	from LinkedList import LinkedList
	
	class TestLinkedList(unittest.TestCase):
	  """
	  UnitTest Cases for the LinkedList Class.
	  """
	  def test_empty(self):
	    """
	    Checks that an empty list can be created.
	    """
	    A=LinkedList([])
	    self.assertEqual(A.length(),0)
	  
	  def test_single(self):
	    """
	    Checks that a single element list can be created.
	    """
	    A=LinkedList([2])
	    self.assertEqual(A.length(),1)
	
	  def test_multiple(self):
	    """
	    Checks that a list with multiple types can be created.
	    """
	    A=LinkedList(["string",1])
	    self.assertEqual(A.length(),2)
	
	  def test_empty(self):
	    """
	    Checks that a node can be inserted into an empty list.
	    """
	    A,B,C=LinkedList([]),LinkedList([]),LinkedList([])
	    newnode=Node(1)
	
	    with self.assertRaises(Exception):
	      A.add_after(1,newnode)
	
	    B.add_first(newnode)
	    C.add_last(newnode)
	    self.assertEqual(B.length(),1)
	    self.assertEqual(C.length(),1)
	
	  #inserting into single element list 
	  def test_single(self):
	    """
	    Checks that a node can be inserted into a single element list.
	    """
	    A,B,C=LinkedList([1]),LinkedList([1]),LinkedList([1])
	    newnode=Node(1)
	    A.add_after(1,newnode)
	    B.add_first(1,newnode)
	    C.add_last(1,newnode)
	
	    self.assertEqual(A.length(),2)
	    self.assertEqual(B.length(),2)
	    self.assertEqual(C.length(),2)
	
	  #inserting into list with no match for target node
	  def test_single(self):
	    """
	    Checks that an Exception is raised when attempting to insert a node 
	    after data that doesn't exist in the list.
	    """
	    A=LinkedList([1,2])
	    newnode=Node(1)
	    with self.assertRaises(Exception):
	      A.add_after(5,newnode)
	
	  #deleting from empty list
	  def test_delete_empty(self):
	    """
	    Checks that an Exception is raised when attempting to delete a node 
	    from an empty list.
	    """
	    A=LinkedList([])
	    with self.assertRaises(Exception):
	        A.remove_node(5)
	
	  #deleting from single element list
	  def test_delete_single(self):
	    """
	    Checks that a node can be deleted from an empty list.
	    """
	    A=LinkedList([1])
	    A.remove_node(1)
	    self.assertEqual(A.length(),0)
	
	  #deleting from multi-element list 
	  def test_delete_multi(self):
	    """
	    Checks that a node can be deleted from a multi-element list.
	    """
	    A=LinkedList([1,6,8,4,9,5])
	    A.remove_node(1)
	    self.assertEqual(A.length(),5)
	
	  #deleting from list with multiple matches for target data 
	  def test_multimatch(self):
	    """
	    Checks that a node can be deleted from a list with multiple
	    matches for the target data.
	    """
	    A=LinkedList([1,2,7,2,9])
	    A.remove_node(2)
	    self.assertEqual(A.length(),4)
	
	  #trying to delete a node with no data match in list
	  def test_delete_nomatch(self):
	    """
	    Checks that an Exception is raised when attempting to delete
	    a node that is not in the list.
	    """
	    A=LinkedList([1,2,7,2,9])
	    with self.assertRaises(Exception):
	      A.remove_node(6)
	
	  #check list length 
	  def test_length(self):
	    """
	    Checks that the list length is correct.
	    """
	    A,B,C=LinkedList([1,2,7,2,9]),LinkedList([]),LinkedList([1])
	    self.assertEqual(A.length(),5)
	    self.assertEqual(B.length(),0)
	    self.assertEqual(C.length(),1)
	
	  def test_node_next(self):
	    """
	    Checks that each node in linked list refers to the correct
	    "next" node.
	    """
	
	    A=LinkedList([5,2,7,3,1])
	    list = []
	    for n in A:
	      list.append(n.get_Data())
	    self.assertEqual(list,[5,2,7,3,1])
	
	  def test_node_initial(self):
	    """
	    Checks a node contains the data field being represented.
	    """
	    N = Node(5)
	    self.assertEqual(N.get_Data(),5)	
