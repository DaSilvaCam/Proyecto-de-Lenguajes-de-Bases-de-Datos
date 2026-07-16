@GetMapping
public String listar(Model modelo,
        @RequestParam(required = false) String buscar,
        @RequestParam(required = false) String categoria) 
        