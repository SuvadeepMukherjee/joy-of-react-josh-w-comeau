# Chap 7 | Dynamic Key Generation

## Dynamic Key Generation

When iterating over data with `.map`, we need to give each React element a unique `key` attribute so that React knows which DOM operations to trigger between renders.

In the earlier examples, our data conveniently came with unique tokens for each item, and we were able to use those tokens:

```jsx
const inventory = [
  {
    id: 'abc123',
    name: 'Soft-boiled egg press',
  },
  {
    id: 'def456',
    name: 'Hello Kitty toothbrush',
  },
];

// We can use the `id` field in our iterations:
inventory.map(item => (
  <ShopItem key={item.id} item={item} />
));
```

But what if our data doesn't *have* a unique token we can use?

In fact, this is one of the most common questions that developers have. React *needs* a unique value for each item, but we don't always have one!

Let's explore with an example.





**The goal is to fix the key warning by dynamically generating the keys for each sticker**

IF we dont unique identifiers for the keys we can add unique identifier on the `onClick` itself 

```jsx
      onClick={(event) => {
        const stickerData = getSticker();
        const newSticker = {
          ...stickerData,
          x: event.clientX,
          y: event.clientY,
          // Come up with a unique value for this sticker.
          id: crypto.randomUUID(),
        };

        const nextStickers = [...stickers, newSticker];
        setStickers(nextStickers);
      }}
```


