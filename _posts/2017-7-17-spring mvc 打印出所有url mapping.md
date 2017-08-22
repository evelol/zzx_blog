    @Autowiredpublic 
    EndpointDocController(RequestMappingHandlerMapping handlerMapping) {
        this.handlerMapping = handlerMapping;
        }
    @RequestMapping(value="/endpointdoc", method=RequestMethod.GET)
    public void show(Model model) {
      model.addAttribute("handlerMethods", this.handlerMapping.getHandlerMethods());
      }