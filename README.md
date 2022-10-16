# LRU-Cache-

## How to implement LRU cache using HashMap and Doubly Linked List

LRU cache stand for Least Recently Used Cache. which evict least recently used entry. As Cache purpose is to provide fast and efficient way of retrieving data. it need to meet certain requirem

<img src="https://user-images.githubusercontent.com/70285394/196023181-ca00ad91-f01f-44a0-bcf5-a3b7906199c3.png" style="background-color:red" />




      data class Entry(
          var value: Int,
          var key: Int,
          var left: Entry?,
          var right: Entry?
      )




      class LRUCache {
          private var hashmap: HashMap<Int, Entry> = HashMap()

          private var start: Entry? = null
          private var end: Entry? = null

          fun getEntry(key: Int): Int {
              val entry = hashmap[key] ?: return -1
              // Key Already Exist, just update
              removeNode(entry)
              addAtTop(entry)
              return entry.value
          }

          fun putEntry(key: Int, value: Int) {
              // Key Already Exist, just update the value and move it to top
              if (hashmap.containsKey(key)) {
                  val entry = hashmap[key]!!
                  entry.value = value
                  removeNode(entry)
                  addAtTop(entry)
              } else {

                  val newNode = Entry(
                      value = value,
                      key = key,
                      left = null,
                      right = null
                  )

                  if (hashmap.size > LRU_SIZE) {
                      hashmap.remove(end?.key)
                      end?.let { removeNode(it) }
                      addAtTop(newNode)
                  } else {
                      addAtTop(newNode)
                  }
                  hashmap[key] = newNode
              }
          }


          private fun addAtTop(node: Entry) {
              node.right = start
              node.left = null
              start?.left = node
              start = node
              if (end == null)
                  end = start

          }

          private fun removeNode(node: Entry) {

              node.left?.let {
                  it.right = node.right
              } ?: kotlin.run { start = node.right }


              node.right?.let {
                  it.left = node.left
              } ?: kotlin.run { end = node.left }
          }

          companion object {
              private const val LRU_SIZE = 4
          }

      }

      val lruCache = LRUCache()
      lruCache.putEntry(1, 1)
      lruCache.putEntry(10, 15)
      lruCache.putEntry(15, 10)
      lruCache.putEntry(10, 16)
      lruCache.putEntry(12, 15)
      lruCache.putEntry(18, 10)
      lruCache.putEntry(13, 16)

      println(lruCache.getEntry(1))      // output -1
      println(lruCache.getEntry(10))     // output 16
      println(lruCache.getEntry(15))     // output 10
      
