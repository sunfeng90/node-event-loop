# node-event-loop
简要学习Node中event-loop的执行过程
## 执行一个循环一共经历六个阶段
- timers
- I/O callbacks
- idle，prepare
- poll
- check
- close callbacks
#### timers
本质：在timers阶段其实使用一个最小堆而不是队列
过程：
   - 从timer堆中取出超时时间最小的原始
   - 如果没有。就退出循环；
   - 如果有。
     - 通过元素计算出handle的地址。
     - 接着判断超时时间四否比循环运行时间还要大，如果大的话，就退出循环。
     - 否则。
       - 移除该handle
       - 判断该handle是不是repeat类型，如果是，就插入堆中，继续执行上面的过程。
       - 否则执行该handle对应的回调函数。
#### I/O callbacks
过程：判断队列是否为空。如果为空，直接退出。如果不为空，就依次取出队列的头节点，然后移除头节点，依次循环，知道队列为空为止。
#### idle和prepare阶段
过程：首先将旧队列转换为新队列。依次循环新队列，每次找到队列首元素指向的handle，然后移除首元素。接着将移除的元素插入旧队列尾部。最后执行对应的回调函数。
#### poll阶段
本质：阻塞等待监听的事件来临，然后执行对应的回调函数。
#### check
#### close阶段