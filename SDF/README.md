# What's the fastest way to author some new layers?

Using the SDF, it has been found to be the more efficient option over using USD directly. This is mostly due to
USD constantly composing as the layers are changed.

# Show me how to create a layer, add a Prim, a few attributes and a variantset!

Creating layers using the SDF API is more of a bottom up where you need to directly create a Prim object passing 
in the Layer object into it's creation. With the USD API, you can create a Prim straight from using the Stage's API.

    from pxr import SDF
    # Create an anonymous layer 
    layer = SDF.Layer.CreateAnonymous()
    # Add sublayers to the layer
    layer.subLayerPaths = ["/sublayer.usda"]
    # Create a prim called 'foo' that is a Def(SDF.SpecifierDef)
    prim = SDF.PrimSpec(layer, "foo", SpecifierDef)
    # add a reference
    prim.referenceList.Add(Sdf.reference("/some/reference.usda"))
    # Create an attribute of type Asset other types can be found in USD repo within wrapTypes.cpp
    attr = SDF.attributeSpec(prim, 'attr_bar', Sdf.ValueTypeNames.Asset)
    # Set the value of the attributes
    attr.default = "/foo/assetattr.usda"
    # Now lets create the VariantSet, a few variants abd set the selection of the variant.
    vs = SDF.VariantSetSpec(prim, "variantset_name") 
    # creates VariantSet
    SDF.VariantSpec(vs, "variant_1") 
    # Creates Variant 1
    SDF.VariantSpec(vs, "variant_2")
    # Creates Variant 2
    vsps = vs.variants["variant_1"]
    # Retrieve variant 1 from the variant set
    vsps.PrimSpec.referenceList.Add(SDF.Reference("/bar/reference.usda"))
    # add a reference to variant 1
    prim.variantSelections['variantset_name'] = 'variant_1'
    # Set the selection to the first variant_1, setting this to an empty string removed selection.

