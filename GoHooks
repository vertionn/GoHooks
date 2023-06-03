package gohooks

import (
	"bytes"
	"encoding/json"
	"fmt"
	"net/http"
	"strconv"
)

type Webhook struct{}

type Message struct {
	Content string  `json:"content,omitempty"`
	Embeds  []Embed `json:"embeds,omitempty"`
}

type Embed struct {
	Title       string         `json:"title,omitempty"`
	Description string         `json:"description,omitempty"`
	URL         string         `json:"url,omitempty"`
	Color       int            `json:"color,omitempty"`
	Fields      []EmbedField   `json:"fields,omitempty"`
	Footer      EmbedFooter    `json:"footer,omitempty"`
	Thumbnail   EmbedThumbnail `json:"thumbnail,omitempty"`
	Image       EmbedImage     `json:"image,omitempty"`
	Author      EmbedAuthor    `json:"author,omitempty"`
	Timestamp   string         `json:"timestamp,omitempty"`
}

type EmbedField struct {
	Name   string `json:"name,omitempty"`
	Value  string `json:"value,omitempty"`
	Inline bool   `json:"inline,omitempty"`
}

type EmbedFooter struct {
	Text    string `json:"text,omitempty"`
	IconURL string `json:"icon_url,omitempty"`
}

type EmbedThumbnail struct {
	URL string `json:"url,omitempty"`
}

type EmbedImage struct {
	URL string `json:"url,omitempty"`
}

type EmbedAuthor struct {
	Name    string `json:"name,omitempty"`
	URL     string `json:"url,omitempty"`
	IconURL string `json:"icon_url,omitempty"`
}

type Builder struct {
	embed Embed
}

func NewEmbed() *Builder {
	return &Builder{
		embed: Embed{},
	}
}

func (b *Builder) SetURL(url string) *Builder {
	b.embed.URL = url
	return b
}

func (b *Builder) SetTitle(title string) *Builder {
	b.embed.Title = title
	return b
}

func (b *Builder) SetDescription(description string) *Builder {
	b.embed.Description = description
	return b
}

func (b *Builder) SetColor(color string) *Builder {
	colors, err := HexToInt(color)
	if err != nil {
		fmt.Println(err.Error())
	}
	b.embed.Color = colors
	return b
}

func (b *Builder) AddField(name, value string, inline bool) *Builder {
	field := EmbedField{
		Name:   name,
		Value:  value,
		Inline: inline,
	}
	b.embed.Fields = append(b.embed.Fields, field)
	return b
}

func (b *Builder) SetFooter(text string, iconURL string) *Builder {
	b.embed.Footer.Text = text
	b.embed.Footer.IconURL = iconURL
	return b
}

func (b *Builder) SetThumbnail(url string) *Builder {
	b.embed.Thumbnail.URL = url
	return b
}

func (b *Builder) SetImage(url string) *Builder {
	b.embed.Image.URL = url
	return b
}

func (b *Builder) SetAuthor(name string, url string, iconURL string) *Builder {
	b.embed.Author.Name = name
	b.embed.Author.URL = url
	b.embed.Author.IconURL = iconURL
	return b
}

func (b *Builder) SetTimestamp(timestamp string) *Builder {
	b.embed.Timestamp = timestamp
	return b
}

func (b *Builder) Build() Embed {
	return b.embed
}

func HexToInt(hex string) (int, error) {
	// Remove the "#" prefix if present
	if len(hex) > 0 && hex[0] == '#' {
		hex = hex[1:]
	}

	// Convert hexadecimal to decimal
	decimal, err := strconv.ParseInt(hex, 16, 64)
	if err != nil {
		return 0, err
	}

	return int(decimal), nil
}

func SendWebhook(webhookURL string, message Message) error {
	payload, err := json.Marshal(message)
	if err != nil {
		return err
	}

	_, err = http.Post(webhookURL, "application/json", bytes.NewBuffer(payload))
	if err != nil {
		return err
	}

	return nil
}
