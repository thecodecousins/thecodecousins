---
title: 'Đóng góp bài viết cho TheCodeCousins'
description: 'Hướng dẫn đóng góp cho TheCodeCousins'
keywords: ['blog', 'software', 'phần-mềm', 'công-nghệ', 'code']
---

Đầu tiên, cảm ơn bạn vì đã muốn chia sẻ ý tưởng, hiểu biết, và quan điểm của mình 🎉🎉 Bạn là một người tuyệt vời và chúng tôi yêu bạn 😍😍

❤️🧡💛💚💙💜🖤

Dưới đây là một số hướng dẫn về cách đóng góp bài viết cho blog của chúng tôi.
Chúng tôi đã cố gắng hết sức để tránh thiếu sót, nhưng nếu bạn nghĩ có điều gì cần bổ sung, đừng ngần ngại góp ý cho chúng tôi bằng một pull request.

### Mục lục

> [Đóng góp bằng bài viết](#author)
>
> [Đóng góp bằng dịch thuật](#translator)
>
> [Bonus: Mục tác giả kèm metadata](#bonus)
>
> [Hỏi đáp thắc mắc](#question)

### <a name="author" id="author"></a> I. Đóng góp bằng bài viết

Hướng dẫn đóng góp bài viết cho [TheCodeCousins](https://thecodecousins.com)

##### Thiết lập môi trường làm việc

Bắt đầu bằng việc fork [repo này](https://github.com/thecodecousins/thecodecousins) của chúng tôi về máy tính của bạn

```bash
git clone https://github.com/YOUR-USERNAME/thecodecousins --recursive
cd thecodecousins/
```

Kiểm tra xem bạn đã cài [hugo](https://gohugo.io) chưa bằng cách chạy `hugo version`.
Nếu mà bạn chưa cài [hugo](https://gohugo.io), hãy xem hướng dẫn cài đặt và cách sử dụng trên [trang của họ](https://gohugo.io/getting-started/installing/).

##### Viết bài

Tạo một bài mới từ mẫu trong chủ đề bằng cách chạy

```bash
hugo new posts/YOUR-POST-NAME/index.vi.md # Chúng tôi sẽ giải thích vì sao dùng index.vi.md trong phần sau
```

Điền vào các mục:

- title: chủ để của bài - hãy viết nó ngắn và hay.
- date: bạn phải dùng [định dạng ISO-8601](https://vi.wikipedia.org/wiki/ISO_8601), hãy dùng [máy tạo dấu thời gian](https://timestampgenerator.com/) này để theo đúng tiêu chuẩn một cách dễ dàng.
- author: tên hoặc biệt hiệu của bạn. Nếu bạn muốn một bút ký hay hơn, hãy làm theo hướng dẫn ở [phần sau](#bonus)
- cover: bức ảnh sẽ được hiển thị ở trang chủ và đầu bài viết của bạn.
- tags: những chủ đề chính của bài
- keywords: như trên nhưng cụ thể hơn
- description: tóm tắt ngắn gọn của bài, sẽ được hiển thị ở trang chủ
- showFullContent: hãy để nó là false để dành diện tích trên trang chủ cho nhiều tác giả khác.

Viết bài của bạn bằng markdown với các tính năng bổ sung từ [hugo](https://gohugo.io/).
Nếu bạn không chỉ muốn dùng markdown (md) cơ bản, xem thêm [bản hướng dẫn của hugo](https://gohugo.io/content-management/).

##### Assets tĩnh

Hệ thống file bài viết trên [TheCodeCousins](https://thecodecousins.com) được sắp xếp theo dạng "leaf bundle", đây cũng là lí do mà file bài viết được để trong tệp với tên bài và có tên file là `index.vi.md`.
Sau khi làm các bước trên, bạn sẽ thấy tệp bài viết của mình như ví dụ sau

![Example source tree](/example-tree.png)

Assets của bạn sau đó có thể được thêm vào bài viết bằng đường link đầy đủ với `content` là tệp gốc
(ví dụ, `/posts/test-posts/img/cover.jpg` là đường link chỉ đến file `cover.jpg` như trong ví dụ trên).

##### Nhắn cho chúng tôi 🎉🎉

Sau khi hoàn thành bài viết, hãy mở một [Pull Request](https://github.com/thecodecousins/thecodecousins/compare) trên repo của chúng tôi và bài viết của bạn sẽ nhanh chóng được đăng. 🥳🥳

### <a name="translator" id="translator"></a> II. Đóng góp bằng dịch thuật

Hướng dẫn đóng góp bài dịch cho [TheCodeCousins](https://thecodecousins.com)

##### Xin ý kiến từ tác giả gốc

Mở [một issue](https://github.com/thecodecousins/thecodecousins/issues/new) với tiêu đề `translation` với đường link trong phần description tới bài viết gốc trên [repo của chúng tôi](https://github.com/thecodecousins/thecodecousins).
Chúng tôi sẽ liên lạc với tác giả gốc và họ cho phép bài được dịch 👌👌.

##### Dịch bài

Xác định ID của bài viết bằng cách nhìn vào tên của tệp chứa (v.d. nếu mà đường dẫn đến file markdown là `/content/posts/example/index.md` thì ID của bài viết sẽ là `example`)
Tạo bài viết bằng ngôn ngữ mới bằng câu lệnh sau với key của ngôn ngữ lấy từ [file config của chúng tôi](https://github.com/thecodecousins/thecodecousins/blob/master/config.yaml) (v.d. `vi` cho tiếng Việt)

```bash
# For English as target language, there's no need for language key
# as English is the default language
hugo new posts/<POST-ID>/index.<LANGUAGE-KEY>.md
```

Điền vào các mục:

- title: dịch từ bài gốc
- date: bạn phải dùng [định dạng ISO-8601](https://vi.wikipedia.org/wiki/ISO_8601), hãy dùng [máy tạo dấu thời gian](https://timestampgenerator.com/) này để theo đúng tiêu chuẩn một cách dễ dàng.
- author: tên hoặc biệt hiệu của bạn. Nếu bạn muốn một bút ký hay hơn, hãy làm theo hướng dẫn ở [phần sau](#bonus)
- cover: lấy từ bài gốc
- tags: lấy hoặc dịch từ bài gốc
- keywords: lấy hoặc dịch từ bài gốc
- description: dịch từ bài gốc
- showFullContent: hãy để nó là false để dành diện tích trên trang chủ cho nhiều tác giả khác.

Mở đầu bài viết với đường link đến bài gốc và nhắc đến tác giả gốc.
Ví dụ, bạn có thể bắt đầu bài viết như sau "Bài viết này được dịch từ [bài viết ví dụ](/vi/posts/example) bởi Stanley Nguyen".

Viết bài của bạn bằng markdown với các tính năng bổ sung từ [hugo](https://gohugo.io/).
Nếu bạn không chỉ muốn dùng markdown (md) cơ bản, xem thêm [bản hướng dẫn của hugo](https://gohugo.io/content-management/).

##### Nhắn cho chúng tôi 🎉🎉

Sau khi hoàn thành bài viết, hãy mở một [Pull Request](https://github.com/thecodecousins/thecodecousins/compare) trên repo của chúng tôi và bài viết của bạn sẽ nhanh chóng được đăng. 🥳🥳

##### <a name="bonus" id="bonus"></a> III. Bonus: Mục tác giả kèm metadata

Phần này dành cho những ai muốn mục `Author` trên bài của mình thêm phần sinh động thay vì một cái string đơn điệu.

Dùng tên tài khoản Github của bạn để tạo một file tác giả để tránh đụng hàng với các tác giả khác.

```bash
touch data/authors/YOUR-USERNAME.yml
```

Có 3 thông tin mà bạn có thể điền trong file tác giả của mình:

- name: cái này chắc ai cũng hiểu 😁😆
- url: đường link đến trang cá nhân của bạn
- bio: giới thiệu ngắn về bản thân, dưới 50 chữ

Sau đó hãy diền tên file vào mục `author` của bài viết. (ví dụ, file tác giả của mình là `stanleynguyen.yml` thì trong bài viết của mình mục `author` sẽ là `stanleynguyen`)

### <a name="question" id="question"> IV. Hỏi đáp thắc mắc

Hăy mở [một issue mới](https://github.com/thecodecousins/thecodecousins/issues/new) với tiêu đề `question` trên [repo của thecodecousins](https://github.com/thecodecousins/thecodecousins).
