## ps

- `docker ps`: liệt kê các container đang chạy. Và option hay dùng với lệnh ps đó là -a
- `docker ps -a`: liệt kê tất cả các container đang có trong hệ thống
- `docker system df`: hiển thị thông tin dung lượng bộ nhớ (disk space) được sử dụng bởi docker bao gồm image, container, volume, cache.

## Docker system

- `docker system events`: hiển thị realtime event từ phía server

- `docker system info`: hiển thị thông tin của toàn hệ thống như số lượng container (đang chạy, dừng), số lượng image, ...

- `docker system prune`: xóa bỏ những thành phần không sử dụng.

## Prune

- `docker system prune` : loại bỏ tất cả container, image, network, và build cache không được sử dụng.

- `docker system prune -a --volumes` : loại bỏ tất cả container, image, network, build cache và volume không được sử dụng.

## Image

- `docker image build`: thực hiện build image từ Dockerfile

- `docker image history` IMAGE: hiển thị history của một image như thời gian tạo, kích thước và cách image được tạo ra.

- `docker image inspect` IMAGE: hiển thị thông tin chi tiết của một image, đặc biệt là các layer tạo ra nó.

- `docker image ls`: hiển thị danh sách các image (có thể dùng docker images thay thế) với các thông tin về tên image, tag, image id, size và thời gian tạo

- `docker image prune`: xóa bỏ các image không sử dụng (dangling image)

- `docker image push`: Push image của bạn lên docker registry. Để làm được điều này bạn cần phải thực hiện docker login trước

- `docker image rm`: xóa bỏ một hoặc nhiều image được chỉ định.

## Build

- `docker image build [OPTIONS] PATH | URL | -`

- Cấu trúc mà mình hay sử dụng với lệnh docker build đó là:
  `docker image build -t my_image:my_tag`
  trong đó option -t (--tag) dùng để đặt tên và tag cho image (theo format image_name:image_tag) chú ý là có dấu . ở cuối lệnh nhé. Dấu . để báo cho docker biết rằng sẽ build image từ Dockerfile ở folder hiện tại.

## Container

- `docker container create` IMAGE: khởi tạo container từ một image

- `docker container inspect` CONTAINER: hiển thị chi tiết thông tin của container

- `docker container logs` CONTAINER: hiển thị logs của một container

- `docker container ls`: liệt kê tất cả container hiện có

- `docker container prune`: xóa bỏ tất cả các container không hoạt động

- `docker container rm`: xóa bỏ một hoặc nhiều container

- `docker container run` IMAGE: khởi tạo và chạy một container từ một image

- `docker container start`: khởi chạy container

- `docker container stop`: chấm dứt hoạt động của container đang chạy.

## Create, start và run

- Đây là 3 lệnh thường sử dụng khi bắt đầu làm việc với container. Để thực thi một container thì bạn cần khởi tạo container từ image và chạy nó. Để làm được điều này bạn có 2 cách:
- Cách 1: dùng docker container create để tạo container từ image và dùng docker container start để khởi chạy.

  - `docker container create --name CONTAINER_NAME IMAGE`

  - `docker container start CONTAINER`

- Cách 2: dùng docker run để tạo container từ image và chạy nó

  - `docker container run --name CONTAINER_NAME -d IMAGE --rm`
  - `option --rm` tự động xóa container ngay sau khi nó stop
  - `option -d` chạy container dưới chế độ background

## Stop và kill

- Có 2 cách để dừng một container đang chạy đó là dùng lệnh docker container stop và lệnh docker container kill.

  - `docker container stop CONTAINER` : mặc định cung cấp 10 giây để container có thể hoàn thành nốt các task của nó trước khi dừng hoạt động.

  - `docker container kill CONTAINER` : dừng hoạt động ngay lập tức.

- Cũng tùy theo mục đich nhưng mình hầu như chỉ xài mỗi lệnh stop. Bạn hoàn toàn có thể thay đổi giá trị 10 giây mặc định với option `--time/ -t`

## Compose

- Về docker compose thì nó sẽ hơi khác một chút đó là dùng Docker Compose CLI thay vì Docker CLI. Nghĩa là thay vì bắt đầu bằng docker thì nó bắt đầu bằng docker-compose. Các lệnh thường sử dụng với docker compose đó là:

  - `docker-compose build`: dùng để build tất cả container được định nghĩa trong compose file

  - `docker-compose up`: thực hiện tạo và khởi chạy các container

  - `docker-compose down`: dùng để dừng các container và xóa hết những gì được tạo từ lệnh up
