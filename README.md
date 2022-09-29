# Id3vx

[![hex.pm](https://img.shields.io/hexpm/v/id3vx.svg)](https://hex.pm/packages/id3vx)

A library for reading and writing ID3 tags. Read [the blog post](https://changelog.com/posts/id3vx-a-library-for-parsing-and-encoding-id3-tags)!

Docs can be found at <https://hexdocs.pm/id3vx>.

It currently supports only ID3v2.3. It specifically also supports Chapters and Table of Contents as it was created to support podcast chapters as a specific usage.

This library development was funded and open-sourced by [Changelog Media](https://changelog.com).

## Installation

Add this to your `mix.exs`:

```elixir
def deps do
  [
    {:id3vx, "~> 0.0.1"}
  ]
end
```

Then run `mix deps.get` to fetch it into your `deps`.

## Examples

### Parse from file

```elixir
{:ok, tag} = Id3vx.parse_file("test/samples/beamradio32.mp3")
```

### Encode new tag

Creating tags is most easily done with the utilities in `Id3vx.Tag`.

```elixir
Id3vx.Tag.create(3)
|> Id3vx.Tag.add_text_frame("TIT1", "Title!")
|> Id3vx.encode_tag()
```

### Parse from binary

```elixir
tag = Id3vx.Tag.create(3)
      |> Id3vx.Tag.add_text_frame("TIT1", "Title!")
tag_binary = Id3vx.encode_tag(tag)
{:ok, tag} = Id3vx.parse_binary(tag_binary)
```

### Replace a tag with a new one

```elixir
tag = Id3vx.Tag.create(3)
      |> Id3vx.Tag.add_text_frame("TIT1", "Title!")

Td3vx.replace_tag(tag, "test/samples/beamradio32.mp3", "test/samples/beamradio32-retagged.mp3")
```

### Add Chapter to an existing ID3 tag

A Chapter often has a URL and image. You can use `Id3vx.Tag.add_attached_picture` for the picture.

```elixir
tag =
  "test/samples/beamradio32.mp3"
  |> Id3vx.parse_file!()
  |> Id3vx.Tag.add_typical_chapter_and_toc(0, 60_000, 0, 12345,
    "A Great Title",
    fn chapter ->
      Id3vx.Tag.add_custom_url(
        chapter,
        "chapter url",
        "https://underjord.io"
      )
    end
  )
```

## Real-world usage

You can see this library being used in the following real-world projects:

- [changelog.com](https://changelog.com) ([source](https://github.com/thechangelog/changelog.com) | [usage](https://github.com/thechangelog/changelog.com/blob/master/lib/changelog/kits/mp3_kit.ex))

Are you using `id3vx`? [Add your project](https://github.com/thechangelog/id3vx/pulls)!
