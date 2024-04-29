pandoc是文档转换利器。在将docx文档转为md文档过程中，

如果直接输入pandoc -o f.md f\[^f\].docx,会丢失word文档中的图片。

输入`Pandora -o f.md f.docx —extract-media=f` 则不会丢失图片。