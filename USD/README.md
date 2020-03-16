# How can I traverse just the Def prims in my Stage?

depth first search on the USD scene.

    [x for x in stage.Traverse()]

The Traverse method filters out all the Prims except for the ones that are specified as Def.



# Ok, but how do I traverse over all Prims in my Scene?

By "All" that means Classes, Defs and Over prims.

    [x for x in stage.TraverseAll()]



# How do I find the ShotRange in my scene?

Our shotRanges are currently a TypedSchema and are composed just below the shot's root prim. Favouring a breadth first search is the more efficient approach here so the Traverse() wouldn't be recommended:


    def breadthFirstSearchStage(parentPrim):
        queue = collections.deque()
        queue.extend(parentPrim.nameChildren)
    
        while(len(queue) != 0):
            currPrim = queue.popleft()
            yield currPrim
    queue.extend(currPrim.nameChildren)

    for found in breadthFirstSearchStage(stage.GetRootLayer().pseudoRoot):
        print found



# How do I create Kinds on the fly in Python?

It seems like you can't do this. The API provided to Python for kinds are an introspect only.

from pxr import Kind
print Kind.Registry.GetAllKinds()



# I want to do more fancy things when traversing my stage!

I recommend a read of https://graphics.pixar.com/usd/docs/Traversing-a-stage.html




# How can I introspect a schema's defaults?

Defaults for all schemas types are stored in the SchemaRegistry in the schematics member which is just an SDFLayer.

    p = Usd.SchemaRegistry.GetSchematics().GetPrimAtPath("/EditFrameRange") # Get Schema for
    a = p.attributes['endTailFrame']
    print a.default

if your defaults aren't in the schematics then the either the package containing your schema plgin isn't in your environment or you haven't correctly set up your resources path in your plugInfo to point where the generatedSchemas.usda file is installed. This generatedSchemas.usda file contains the default.
