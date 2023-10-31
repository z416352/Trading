# Deployment
需要先在 K8s 中建立一個 secrets 存放 mongodb atlas 的 `password`, `username`, `AtlasInfo`.
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mongodb-secret
type: Opaque
data:
  mongo-root-username: xxx
  mongo-root-password: xxx
  Atlas_Info: xxx
```

部屬所需要的 images 可以使用 Artifact Registry 來儲存

* crawler-deployment.yaml:
    * 下面這幾個環境參數需要根據 db-api-service.yaml 的設定更改
      * `db_API_ServiceName`
      * `db_API_ServicePort`
      * `db_API_Namespace`
    * 其中的 image 來源需要修改成自己存放的位置
* db-api-deployment.yaml:
    * 下面的參數會從 k8s 中的 secrets 中調取
      * `MONGODB_USERNAME`
      * `MONGODB_PASSWORD`
      * `Atlas_Info`
    * 其中的 image 來源需要修改成自己存放的位置
 
      
如果發現 crawler 無法從 db-api 取得資料，可能是 Database-api/internal/gin_services/middleware.go 中的白名單 IP 擋住了 Pod IP。可以查看 GKE 或 K8s 所分配的 Pod IP range 做調整。

