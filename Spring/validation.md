### Binding Result란?
    
    스프링이 제공하는 검증 오류를 보관하는 객체. 검증 오류가 발생하면 new FieldError() 혹은 new ObjectError() 이런 식으로 오류를 생성해서 이 객체에 보관하면 된다.
    @ModelAttribute에 데이터 바인딩 시 오류가 발생하면 오류 정보(FieldError)를 BindingResult에 담기고 컨트롤러가 정상 호출된다.
    

### Validator란?
     
    Validatior없이 검증할 때는 controller 안에서 validation 코드가 복잡하게 섞여 있으므로 따로 Validator라는 클래스로 빼주는 것이 좋다. 
    그리고 Spring에서 제공하는 Validator 인터페이스를 이용해 Validator를 구현하고 @InitBinder를 이용해 WebDataBinder에 Validator를 등록해두면 단순히 controller 파라미터에 @Validated를 써주는 것만으로 Validation을 적용할 수 있다.
    
### Bean Validation이란?
    
    public class Item {
     private Long id;
    
     @NotBlank
     private String itemName;
    
     @NotNull
     @Range(min = 1000, max = 1000000)
     private Integer price;
    
     @NotNull
     @Max(9999)
     private Integer quantity;
     //...
    }
    
    자바에서 제공하는 이런 검증 로직을 모든 프로젝트에 적용할 수 있게 공통화하고 표준화 한것. 
    Bean Validation을 잘 활용하면 애노테이션 하나로 검증 로직을 매우 편리하게 적용할 수 있다.
    
    Bean Validation은 특정한 구현체가 아니라 Bean Validation 2.0(JSR-380)이라는 기술 표준이다.
    쉽게 이야기해서 검증 애노테이션과 여러 인터페이스의 모음이다. 마치 JPA가 표준 기술이고 그 구현체로 하이버네이트가 있는 것과 같다.
    
### 스프링에서 Bean Validation을 어떻게 사용하게 해줄까?
    
    import javax.validation.ValidatorFactory;
    import java.util.Set;
    public class BeanValidationTest {
     @Test
     void beanValidation() {
    	 ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
    	 Validator validator = factory.getValidator(); // 검증기 얻기
    	
    	 //
    	 Item item = new Item();
    	 item.setItemName(" "); //공백
    	 item.setPrice(0);
    	 item.setQuantity(10000);
    
    	 Set<ConstraintViolation<Item>> violations = validator.validate(item);
    	 for (ConstraintViolation<Item> violation : violations) {
    		 System.out.println("violation=" + violation);
    		 System.out.println("violation.message=" + violation.getMessage());
    	 }
     }
    }
    
    Spring없이 순수하게 위와 같이 검증을 수행할 수 있다.
    
    - 스프링 MVC는 어떻게 Bean Validator를 사용?
    
    스프링 부트가 spring-boot-starter-validation 라이브러리를 넣으면 자동으로 Bean Validator를 인지하고 스프링에 통합한다. 
    
    - 스프링 부트는 자동으로 글로벌 Validator로 등록한다.

    LocalValidatorFactoryBean 을 글로벌 Validator로 등록한다. 이 Validator는 @NotNull 같은 애노테이션을 보고 검증을 수행한다. 이렇게 글로벌 Validator가 적용되어 있기 때문에, @Valid ,@Validated 만 적용하면 된다.
    검증 오류가 발생하면, FieldError , ObjectError 를 생성해서 BindingResult 에 담아준다
    
    ---
    
    김영한, 인프런, 스프링 Web MVC 2편, Bean Validation - 스프링 적용