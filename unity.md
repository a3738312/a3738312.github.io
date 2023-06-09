* RectTransform 设置UI宽高，使用 SetSizeWithCurrentAnchors 方法，直接使用 sizeDelta 设置大多数时候无法达到预期效果。
设置对齐的时候，使用 offsetMax 和 offsetMin 
具体介绍： https://blog.csdn.net/LLLLL__/article/details/121412067

* UI点击穿透可以使用以下方法：
    ```C#
        //点击事件穿透
        public void OnPointerClick(PointerEventData eventData)
        {
            PassEvent(eventData, ExecuteEvents.pointerClickHandler);
        }
        //按下事件穿透
        public void OnPointerDown(PointerEventData eventData)
        {
            PassEvent(eventData, ExecuteEvents.pointerDownHandler);
        }
        //抬起事件穿透
        public void OnPointerUp(PointerEventData eventData)
        {
            PassEvent(eventData, ExecuteEvents.pointerUpHandler);
        }
        //拖动事件穿透
        public void OnDrag(PointerEventData eventData)
        {
            PassEvent(eventData, ExecuteEvents.dragHandler);
        }
        private void PassEvent<T>(PointerEventData data, ExecuteEvents.EventFunction<T> function)
            where T : IEventSystemHandler
        {
            List<RaycastResult> results = new List<RaycastResult>();
            EventSystem.current.RaycastAll(data, results);
            GameObject current = data.pointerCurrentRaycast.gameObject;
            for (int i = 0; i < results.Count; i++)
            {
                if (current != results[i].gameObject)
                {
                    Debug.Log("透下去的物体：" + results[i].gameObject.name);
                    ExecuteEvents.Execute(results[i].gameObject, data, function);
                    //RaycastAll后ugui会自己排序，如果你只想响应透下去的最近的一个响应，这里ExecuteEvents.Execute后直接break就行。
                }
            }
        }
    //给需要点击穿透的类添加继承 
    //IPointerClickHandler, IPointerDownHandler, IPointerUpHandler, IDragHandler
    ```
