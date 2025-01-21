### 予定の構成
```
student-management/
├── api/                # gRPCの.protoファイルを格納
├── cmd/                # メインアプリケーションのエントリーポイント
├── internal/           # アプリケーションのビジネスロジック
│   ├── student/        # 受講生管理に関するロジック
│   ├── course/         # コース管理に関するロジック
├── pkg/                # 再利用可能なユーティリティコード
├── go.mod              # Goモジュールファイル
├── go.sum              # 依存関係管理ファイル
└── README.md           # プロジェクト説明
```

### 導入手順
- gRPCのprotoファイルを定義
```api/student.proto
syntax = "proto3";

package student;

option go_package = "student-management/api;api";

service StudentService {
  rpc CreateStudent(CreateStudentRequest) returns (CreateStudentResponse);
  rpc GetStudent(GetStudentRequest) returns (GetStudentResponse);
  rpc UpdateStudent(UpdateStudentRequest) returns (UpdateStudentResponse);
  rpc DeleteStudent(DeleteStudentRequest) returns (DeleteStudentResponse);
  rpc ListStudents(ListStudentsRequest) returns (ListStudentsResponse);
}

message Student {
  string id = 1;
  string name = 2;
  string email = 3;
  repeated string courses = 4;
}

message CreateStudentRequest {
  string name = 1;
  string email = 2;
}

message CreateStudentResponse {
  Student student = 1;
}

message GetStudentRequest {
  string id = 1;
}

message GetStudentResponse {
  Student student = 1;
}

message UpdateStudentRequest {
  string id = 1;
  string name = 2;
  string email = 3;
  repeated string courses = 4;
}

message UpdateStudentResponse {
  Student student = 1;
}

message DeleteStudentRequest {
  string id = 1;
}

message DeleteStudentResponse {
  bool success = 1;
}

message ListStudentsRequest {}

message ListStudentsResponse {
  repeated Student students = 1;
}
```

### gRPCコード生成
以下のコマンドでgRPCコードを生成します：
必要なツールをインストール
```
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```
コード生成を実行
```
protoc --go_out=. --go-grpc_out=. api/student.proto
```


