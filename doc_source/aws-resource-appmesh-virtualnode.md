# AWS::AppMesh::VirtualNode<a name="aws-resource-appmesh-virtualnode"></a>

Creates a virtual node within a service mesh\.

A virtual node acts as a logical pointer to a particular task group, such as an Amazon ECS service or a Kubernetes deployment\. When you create a virtual node, you must specify the DNS service discovery hostname for your task group\.

Any inbound traffic that your virtual node expects should be specified as a `listener`\. Any outbound traffic that your virtual node expects to reach should be specified as a `backend`\.

The response metadata for your new virtual node contains the `arn` that is associated with the virtual node\. Set this value \(either the full ARN or the truncated resource name: for example, `mesh/default/virtualNode/simpleapp`\) as the `APPMESH_VIRTUAL_NODE_NAME` environment variable for your task group's Envoy proxy container in your task definition or pod spec\. This is then mapped to the `node.id` and `node.cluster` Envoy parameters\.

**Note**  
If you require your Envoy stats or tracing to use a different name, you can override the `node.cluster` value that is set by `APPMESH_VIRTUAL_NODE_NAME` with the `APPMESH_VIRTUAL_NODE_CLUSTER` environment variable\.

## Syntax<a name="aws-resource-appmesh-virtualnode-syntax"></a>

To declare this entity in your AWS CloudFormation template, use the following syntax:

### JSON<a name="aws-resource-appmesh-virtualnode-syntax.json"></a>

```
{
  "Type" : "AWS::AppMesh::VirtualNode",
  "Properties" : {
      "[MeshName](#cfn-appmesh-virtualnode-meshname)" : String,
      "[Spec](#cfn-appmesh-virtualnode-spec)" : [VirtualNodeSpec](aws-properties-appmesh-virtualnode-virtualnodespec.md),
      "[Tags](#cfn-appmesh-virtualnode-tags)" : [ [TagRef](aws-properties-appmesh-virtualnode-tagref.md), ... ],
      "[VirtualNodeName](#cfn-appmesh-virtualnode-virtualnodename)" : String
    }
}
```

### YAML<a name="aws-resource-appmesh-virtualnode-syntax.yaml"></a>

```
Type: AWS::AppMesh::VirtualNode
Properties: 
  [MeshName](#cfn-appmesh-virtualnode-meshname): String
  [Spec](#cfn-appmesh-virtualnode-spec): 
    [VirtualNodeSpec](aws-properties-appmesh-virtualnode-virtualnodespec.md)
  [Tags](#cfn-appmesh-virtualnode-tags): 
    - [TagRef](aws-properties-appmesh-virtualnode-tagref.md)
  [VirtualNodeName](#cfn-appmesh-virtualnode-virtualnodename): String
```

## Properties<a name="aws-resource-appmesh-virtualnode-properties"></a>

`MeshName`  <a name="cfn-appmesh-virtualnode-meshname"></a>
The name of the service mesh to create the virtual node in\.  
*Required*: Yes  
*Type*: String  
*Update requires*: [Replacement](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html#update-replacement)

`Spec`  <a name="cfn-appmesh-virtualnode-spec"></a>
The virtual node specification to apply\.  
*Required*: Yes  
*Type*: [VirtualNodeSpec](aws-properties-appmesh-virtualnode-virtualnodespec.md)  
*Update requires*: [No interruption](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html#update-no-interrupt)

`Tags`  <a name="cfn-appmesh-virtualnode-tags"></a>
Optional metadata that you can apply to the virtual node to assist with categorization and organization\. Each tag consists of a key and an optional value, both of which you define\. Tag keys can have a maximum character length of 128 characters, and tag values can have a maximum length of 256 characters\.  
*Required*: No  
*Type*: List of [TagRef](aws-properties-appmesh-virtualnode-tagref.md)  
*Update requires*: [No interruption](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html#update-no-interrupt)

`VirtualNodeName`  <a name="cfn-appmesh-virtualnode-virtualnodename"></a>
The name to use for the virtual node\.  
*Required*: Yes  
*Type*: String  
*Update requires*: [Replacement](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html#update-replacement)

## Return Values<a name="aws-resource-appmesh-virtualnode-return-values"></a>

### Ref<a name="aws-resource-appmesh-virtualnode-return-values-ref"></a>

 When you pass the logical ID of this resource to the intrinsic `Ref` function, `Ref` returns the resource ARN\. For example:

 `{ "Ref": "myVirtualNode" }` 

When you pass the logical ID of an `AWS::AppMesh::VirtualNode` resource to the intrinsic Ref function, the function returns the virtual node ARN, such as `arn:aws:appmesh:us-east-1:555555555555:virtualNode/myVirtualNode `\.

For more information about using the `Ref` function, see [Ref](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html)\.

### Fn::GetAtt<a name="aws-resource-appmesh-virtualnode-return-values-fn--getatt"></a>

The `Fn::GetAtt` intrinsic function returns a value for a specified attribute of this type\. The following are the available attributes and sample return values\.

For more information about using the `Fn::GetAtt` intrinsic function, see [Fn::GetAtt](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getatt.html)\.

#### <a name="aws-resource-appmesh-virtualnode-return-values-fn--getatt-fn--getatt"></a>

`Arn`  <a name="Arn-fn::getatt"></a>
The full Amazon Resource Name \(ARN\) for the virtual node\.

`MeshName`  <a name="MeshName-fn::getatt"></a>
The name of the service mesh that the virtual node resides in\.

`Uid`  <a name="Uid-fn::getatt"></a>
The unique identifier for the virtual node\.

`VirtualNodeName`  <a name="VirtualNodeName-fn::getatt"></a>
The name of the virtual node\.

## Examples<a name="aws-resource-appmesh-virtualnode--examples"></a>

### Create a Virtual Node<a name="aws-resource-appmesh-virtualnode--examples--Create_a_Virtual_Node"></a>

This example creates a virtual node with two backends and a listener with a health check policy\. It also sends access logs to a file path and uses DNS service discovery\.

#### JSON<a name="aws-resource-appmesh-virtualnode--examples--Create_a_Virtual_Node--json"></a>

```
{
   "Description": "Basic Test Virtual Node",
   "Resources": {
      "BasicVirtualNode": {
         "Type": "AWS::AppMesh::VirtualNode",
         "Properties": {
            "VirtualNodeName": "TestVirtualNode",
            "MeshName": null,
            "Spec": {
               "Backends": [
                  {
                     "VirtualService": {
                        "VirtualServiceName": "Backend_1"
                     }
                  },
                  {
                     "VirtualService": {
                        "VirtualServiceName": "Backend_2"
                     }
                  }
               ],
               "Listeners": [
                  {
                     "HealthCheckPolicy": {
                        "HealthyThreshold": 2,
                        "IntervalMillis": 5000,
                        "Path": "Path",
                        "Port": 8080,
                        "Protocol": "http",
                        "TimeoutMillis": 2000,
                        "UnhealthyThreshold": 2
                     },
                     "PortMapping": {
                        "Port": 8080,
                        "Protocol": "http"
                     }
                  }
               ],
               "ServiceDiscovery": {
                  "DNS": {
                     "Hostname": "Hostname"
                  }
               },
               "Logging": {
                  "AccessLog": {
                     "File": {
                        "Path": "Path"
                     }
                  }
               }
            },
            "Tags": [
               {
                  "Key": "Key1",
                  "Value": "Value1"
               },
               {
                  "Key": "Key2",
                  "Value": "Value2"
               }
            ]
         }
      }
   },
   "Outputs": {
      "VirtualNodeName": {
         "Description": "Name of the VirtualNode",
         "Value": {
            "Fn::GetAtt": [
               "BasicVirtualNode",
               "VirtualNodeName"
            ]
         }
      },
      "MeshName": {
         "Description": "Name of the Mesh",
         "Value": {
            "Fn::GetAtt": [
               "BasicVirtualNode",
               "MeshName"
            ]
         }
      },
      "Arn": {
         "Description": "Arn of the VirtualNode created",
         "Value": {
            "Fn::GetAtt": [
               "BasicVirtualNode",
               "Arn"
            ]
         }
      },
      "Uid": {
         "Description": "Uid of the VirtualNode created",
         "Value": {
            "Fn::GetAtt": [
               "BasicVirtualNode",
               "Uid"
            ]
         }
      }
   }
}
```

#### YAML<a name="aws-resource-appmesh-virtualnode--examples--Create_a_Virtual_Node--yaml"></a>

```
Description: "Basic Test Virtual Node"
Resources:
  BasicVirtualNode:
    Type: "AWS::AppMesh::VirtualNode"
    Properties:
      VirtualNodeName: "TestVirtualNode"
      MeshName: !ImportValue TestMeshName
      Spec:
        Backends:
        - VirtualService:
            VirtualServiceName: "Backend_1"
        - VirtualService:
            VirtualServiceName: "Backend_2"
        Listeners:
        - HealthCheckPolicy:
            HealthyThreshold: 2
            IntervalMillis: 5000
            Path: "Path"
            Port: 8080
            Protocol: "http"
            TimeoutMillis: 2000
            UnhealthyThreshold: 2
          PortMapping:
            Port: 8080
            Protocol: "http"
        ServiceDiscovery:
          DNS:
            Hostname: "Hostname"
        Logging:
          AccessLog:
            File:
              Path: "Path"
      Tags:
      - Key: "Key1"
        Value: "Value1"
      - Key: "Key2"
        Value: "Value2"

Outputs:
  VirtualNodeName:
    Description: Name of the VirtualNode
    Value:
      Fn::GetAtt:
      - BasicVirtualNode
      - VirtualNodeName
  MeshName:
    Description: Name of the Mesh
    Value:
      Fn::GetAtt:
      - BasicVirtualNode
      - MeshName
  Arn:
    Description: Arn of the VirtualNode created
    Value:
      Fn::GetAtt:
      - BasicVirtualNode
      - Arn
  Uid:
    Description: Uid of the VirtualNode created
    Value:
      Fn::GetAtt:
      - BasicVirtualNode
      - Uid
```

## See Also<a name="aws-resource-appmesh-virtualnode--seealso"></a>
+  [Virtual Nodes](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_nodes.html) in the * AWS App Mesh User Guide *\.
+  [CreateVirtualNode](https://docs.aws.amazon.com/app-mesh/latest/APIReference/API_CreateVirtualNode.html) in the * AWS App Mesh API Reference *\.