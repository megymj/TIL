# Cors 해결



## 1. WebMvcConfigure 상속

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry corsRegistry) {
        corsRegistry.addMapping("/**")
//                .allowedOrigins("http://localhost:3000")
                .allowedOriginPatterns("*")
                .allowedMethods("GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .allowCredentials(true)
                .maxAge(3600);
    }

}
```

#### 결과: 여전히 Cors 오류 발생



## 2. Spring Security

* 검색 결과, Spring Security를 프로젝트에서 사용 중이면 1번이 동작하지 않는다고 한다.

```java
@Configuration
@EnableWebSecurity              // Spring Security Filter 가 Spring Filter Chain 에 등록이 된다.
public class SecurityConfig {   // WebSecurityConfigurerAdapter is deprecated

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf().disable();

        // Security 는 기본적으로 세션을 사용
        // 여기서는 세션을 사용하지 않기 때문에 세션 설정을 Stateless 로 설정
        http
                .httpBasic().disable()
                .authorizeRequests()
                .mvcMatchers(HttpMethod.OPTIONS, "/**").permitAll() // Preflight Request 허용해주기
                .and()
                .cors().configurationSource(corsConfigurationSource())
                .and()
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                // form 기반의 로그인에 대해 비활성화
                .formLogin().disable();

        return http.build();
    }

    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();

        configuration.addAllowedOrigin("http://localhost:3000");
        configuration.addAllowedHeader("*");
        configuration.addAllowedMethod("*");
        configuration.setAllowCredentials(true);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }
}
```

#### 실행 결과: 여전히 cors 오류 발생



## 3. Filter 사용

```java
@Component
@Order(Ordered.HIGHEST_PRECEDENCE)
public class CorsFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;

        response.setHeader("Access-Control-Allow-Origin", "http://localhost:3000");
        response.setHeader("Access-Control-Allow-Credentials", "true");
        response.setHeader("Access-Control-Allow-Methods", "*");
        response.setHeader("Access-Control-Max-Age", "3600");
        response.setHeader("Access-Control-Allow-Headers",
                "Origin, X-Requested-With, Content-Type, Accept, Authorization");

        if ("OPTIONS".equalsIgnoreCase(request.getMethod())) {
            response.setStatus(HttpServletResponse.SC_OK);
        } else {
            chain.doFilter(req, res);
        }
    }

    @Override
    public void destroy() {

    }
}
```

#### 실행 결과: cors 이슈가 해결됨



#### 현재는 1번과 3번을 사용하여 CORS를 처리하고 있으나, 실질적으로는 Filter에서 CORS를 처리하고 있다. 

