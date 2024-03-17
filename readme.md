# Dockerfile

- `FROM`: Chỉ định rằng image nào sẽ được dùng làm image cơ sở để quá trình build image thực thiện các câu lệnh tiếp theo. Các image base này sẽ được tải về từ Public Repository hoặc Private Repository riêng của mỗi người tùy theo setup.

- `WORKDIR`: Thiết lập thư mục làm việc trong container cho các lệnh COPY, ADD, RUN, CMD, và ENTRYPOINT.

```
FROM <image> [AS <name>]
FROM <image>[:<tag>] [AS <name>]
FROM <image>[@<digest>] [AS <name>]

```

- `LABEL`: Chỉ thị LABEL được dùng để thêm các thông tin meta vào Docker Image khi chúng được build. Chúng tồn tại dưới dạng các cặp key - value, được lưu trữ dưới dạng chuỗi. Có thể chỉ định nhiều label cho một Docker Image, và tất nhiên mỗi cặp key - value phải là duy nhất. Nếu cùng một key mà được khai báo nhiều giá trị (value) thì giá trị được khai báo gần đây nhất sẽ ghi đè lên giá trị trước đó.

```
LABEL <key>=<value> <key>=<value> <key>=<value> ... <key>=<value>
LABEL com.example.some-label="lorem"
LABEL version="2.0" description="Lorem ipsum dolor sit amet, consectetur adipiscing elit."

```

- Để xem thông tin meta của một Docker Image, ta sử dụng dòng lệnh:

```
docker inspect <image id>

```

- `MAINTAINER`: Chỉ thị MAINTAINER dùng để khai báo thông tin tác giả người viết ra file Dockerfile.

```
MAINTAINER <name> [<email>]
MAINTAINER NamDH <namduong3699@gmail.com>
```

- Hiện nay, theo tài liệu chính thức từ bên phía Docker thì việc khai báo MAINTAINER đang dần được thay thế bằng LABEL maintainer bới tính linh hoạt của nó khi ngoài thông tin về tên, email của tác giả thì ta có thể thêm nhiều thông tin tùy chọn khác qua các thẻ metadata và có thể lấy thông tin dễ dàng với câu lệnh docker inspect.

```
LABEL maintainer="namduong3699@gmail.com"
```

- `RUN`: Chỉ thị RUN dùng để chạy một lệnh nào đó trong quá trình build image và thường là các câu lệnh Linux. Tùy vào image gốc được khai báo trong phần FROM thì sẽ có các câu lệnh tương ứng. Ví dụ, để chạy câu lệnh update đối với Ubuntu sẽ là RUN apt-get update -y còn đối với CentOS thì sẽ là Run yum update -y. Kết quả của câu lệnh sẽ được commit lại, kết quả commit đó sẽ được sử dụng trong bước tiếp theo của Dockerfile.

```
RUN <command>
RUN ["executable", "param1", "param2"]
RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'
-------- hoặc --------
RUN ["/bin/bash", "-c", "echo hello"]
```

- `ADD`: Chỉ thị ADD sẽ thực hiện sao chép các tập, thư mục từ máy đang build hoặc remote file URLs từ src và thêm chúng vào filesystem của image dest.

```
ADD [--chown=<user>:<group>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]

```

- Trong đó:
  - src có thể khai báo nhiều file, thư mục, ...
  - dest phải là đường dẫn tuyệt đối hoặc có quan hệ chỉ thị đối với WORKDIR.

```
ADD hom* /mydir/
ADD hom?.txt /mydir/
ADD test.txt relativeDir/
```

- `COPY`: Chỉ thị COPY cũng giống với ADD là copy file, thư mục từ <src> và thêm chúng vào <dest> của container. Khác với ADD, nó không hỗ trợ thêm các file remote file URLs từ các nguồn trên mạng.

```
COPY [--chown=<user>:<group>] <src>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
```

- `ENV`: Chỉ thị ENV dùng để khai báo các biến môi trường. Các biến này được khai báo dưới dạng key - value bằng các chuỗi. Giá trị của các biến này sẽ có hiện hữu cho các chỉ thị tiếp theo của Dockerfile.

```
ENV <key>=<value> ...
ENV DOMAIN="viblo.asia"
ENV PORT=80
ENV USERNAME="namdh" PASSWORD="secret"
```

- Ngoài ra cũng có thể thay đổi giá trị của biến môi trường bằng câu lệnh khởi động container:

```
docker run --env <key>=<value>
```

- `CMD`: Chỉ thị CMD định nghĩa các câu lệnh sẽ được chạy sau khi container được khởi động từ image đã build. Có thể khai báo được nhiều nhưng chỉ có duy nhất CMD cuối cùng được chạy.

```
CMD ["executable","param1","param2"]
CMD ["param1","param2"]
CMD command param1 param2
```

- `USER`: Có tác dụng set username hoặc UID để sử dụng khi chạy image và khi chạy các lệnh có trong RUN, CMD, ENTRYPOINT sau nó.

```
USER <user>[:<group>]
hoặc
USER <UID>[:<GID>]

FROM alpine:3.4
RUN useradd -ms /bin/bash namdh
USER namdh

```

- `EXPOSE`: thiết lập port để truy cập tới container sau khi đã khởi chạy.
- `ENTRYPOINT`: cung cấp một số lệnh mặc định cùng tham số khi thực thi container.
- `VOLUME` tạo một folder dùng để truy cập và lưu trữ dữ liệu, folder được liên kết từ máy host và container.
- `ARG` Định nghĩa các biến để sử dụng trong build-time.

- `ONBUILD`: tạo một trigger cho image để thực thi khi nó được sử dụng làm base image cho việc build một image khác

- `STOPSIGNAL`: chỉ định kí hiệu hệ thống dùng để stop container.

- `HEALTHCHECK`: cung cấp phương thức cho Docker để kiểm tra container có hoạt động bình thường hay không.

- `SHELL`: dùng để thay đổi các lệnh shell mặc định.

- Sau khi đã nắm được các thành phần của Dockerfile, việc tiếp theo là làm thế nào để viết Dockerfile từ các thành phần đó. Xem lại ví dụ ở trên thì có thể định hình được cấu trúc của 1 Dockerfile sẽ gồm các phần chính:

  - FROM: xác định base image. Lệnh đầu tiên của bất cứ Dockerfile nào.
  - Thiết lập workdir: chỉ rõ thư mục làm việc để copy source và cài ứng dụng chẳng hạn. Việc này rất cần thiết để tách biệt các ứng dụng với nhau
  - Cài đặt ứng dụng: Sau khi đã có source code thì hẳn là run build và start application đúng không nhỉ
  - Tùy chỉnh cấu hình: Đây là bước cuối để chốt lại cái mà bạn sẽ public với container được tạo ra từ image được định nghĩa với Dockerfile. Nào là port, cần chạy thêm command nào. Đây chính là nơi mà các bạn sẽ sử dụng những lệnh như EXPOSE, CMD hay ENTRYPOINT. Và một điều lưu ý là đừng quên HEALTHCHECK đối với một số container quan trọng.
  - Lưu ý khi viết Dockerfile, bạn cũng nên thiết lập .dockerignore để loại bỏ những file ko cần thiết ra khỏi quá trình build. Điều này giúp cho việc bạn không phải build - lại image khi có những thay đổi nhỏ như update README.md...

```
FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY . .
WORKDIR /src/DemoService
RUN dotnet restore
RUN dotnet build

FROM build AS publish
RUN dotnet publish -c Debug -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
EXPOSE 20016
ENTRYPOINT ["dotnet", "DemoService.dll", "--debug"]
```
