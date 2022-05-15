# JVM
## GC - garbage collection
JVM starts 2 threads for garbage collection:
1. Search Thread - for searching garbages - one per JVM
2. GC Thread - for cleaning garbages - can be many per JVM

## Search Thread
There are 2 algorithms used by JVM used to search the dead (garbage) objects:
* **Reference counter (old)** - this algorithm was initially used to search garbage. It is simple to implement and understand, but it does not solve object's circular references. Every time object is used by another object, object's reference is increased or decreased by 1. If the counter is equal to 0, then the object is considered as garbage.
* **Rechability analysis (currently used)** - currently used algorithm by JVM. The algorithm traces the objects by chain of references starting from "root" objects. The "root" is GC root here and the path from root to certain object is called "reference chain". If the object is not reachable from any "root", then the object will be considered as garbage.

## HotSpot Heap Structure
Consists from following regions:
1. **Young generation** - stores small size and newly created objects. It consists from eden and S0/S1 survivor spaces.
    1. **Eden**
    2. **S0**
    3. **S1**
2. **Old generation** - stores big sizes objects existing for long time
3. **Metaspace (aka PermGen in old Java)** - used by JVM for internal purposes
